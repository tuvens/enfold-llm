# Updating Existing Pages

This guide covers updating pages that already exist in WordPress.

## Overview

Existing pages have:
- A content file: `content/pages/<name>.txt`
- A meta file: `meta/pages/<name>.json` (contains the WordPress page ID)

When you push changes to the content file, GitHub Actions uses the page ID from the meta file to update the existing WordPress page.

## Step-by-Step Process

### 1. Locate the Page

Find the content file you want to edit:

```bash
ls content/pages/
```

### 2. Verify Meta File Exists

Check that a corresponding meta file exists:

```bash
cat meta/pages/<page-name>.json
```

Expected format:
```json
{
  "post_id": 12345,
  "title": "Page Title",
  "slug": "page-slug",
  "type": "pages"
}
```

If no meta file exists, check `meta/<page-name>.json` (legacy location).

### 3. Edit the Content File

Make your changes to the content file:

```bash
vim content/pages/<page-name>.txt
```

**Important:** Existing pages typically do NOT have YAML frontmatter. The metadata is stored in the separate JSON file.

### 4. Commit and Push

```bash
git add content/pages/<page-name>.txt
git commit -m "Update <page-name>: describe your changes"
git push origin staging
```

### 5. Verify

1. Check GitHub Actions for successful deployment
2. Visit the staging URL to verify changes
3. Clear SiteGround cache if changes don't appear

## Using str_replace for Precise Edits

When making specific changes, use `str_replace` commands to ensure precision:

```bash
# Example: Change a heading
old_str:
[av_heading heading='Old Heading' tag='h2'

new_str:
[av_heading heading='New Heading' tag='h2'
```

**Tips for str_replace:**
- Include enough context to make the match unique
- Be careful with whitespace - it matters in shortcodes
- Test your match by searching the file first

## Bulk Updates

When updating multiple related pages (e.g., English and Portuguese versions):

1. Make changes to all files
2. Commit them together:

```bash
git add content/pages/forro-events.txt content/pages/eventos-de-forro.txt
git commit -m "Update events pages: add new festival"
git push origin staging
```

## Common Edit Patterns

### Update Text Content

Find the `[av_textblock]` element and modify the HTML inside:

```
[av_textblock size='' font_color='' color='' av_uid='av-xyz']
<p>Your new text here.</p>
[/av_textblock]
```

### Update a Heading

```
[av_heading heading='New Heading Text' tag='h2' ...
```

### Update a Button Link

```
[av_button label='Click Me' link='manually,https://new-url.com' ...
```

### Update an Image

```
[av_image src='https://example.com/wp-content/uploads/new-image.jpg' ...
```

## Testing Workflow

1. **Always test on staging first**
2. Push to `staging` branch
3. Verify at `https://staging.example.com/<page-slug>/`
4. Once verified, merge to `main` for production

```bash
# After staging verification
git checkout main
git merge staging
git push origin main
```

## Troubleshooting

### Changes pushed but page not updated

1. Check GitHub Actions completed successfully
2. Clear SiteGround cache
3. Verify the meta file has the correct page ID

### "Page not found" in WordPress

The page ID in the meta file may be wrong. Verify the ID matches the actual WordPress page.

### Shortcodes render as plain text

- Check that shortcodes are properly closed
- Verify the Enfold REST Meta plugin is active
- Ensure `_aviaLayoutBuilder_active` is set to "active"

### Broken page layout

- Check for unclosed shortcode tags
- Verify column structure (e.g., `av_one_half` pairs must add up)
- Look for missing `first` attribute on first column
