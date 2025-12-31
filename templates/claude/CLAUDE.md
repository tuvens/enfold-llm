# CLAUDE.md - Instructions for Claude Code

This file provides context for Claude Code when working with this repository.

## Repository Purpose

This is a GitOps-managed WordPress site for Your Site using the Enfold theme. Page content is stored as Enfold shortcodes in text files and automatically deployed to WordPress via GitHub Actions.

## Key Architecture

```
Edit content/*.txt → git push → GitHub Actions → WordPress REST API
                                      │
                     ┌────────────────┴────────────────┐
                     │                                 │
              staging branch                    main branch
                     ↓                                 ↓
        staging.example.com                  example.com
```

## File Structure

- `content/*.txt` - Enfold shortcode content (this is what you edit)
- `meta/*.json` - Page metadata with WordPress page IDs
- `scripts/deploy-page.sh` - Deployment script (handles large files)

## How Deployment Works

1. Changes to `content/*.txt` trigger GitHub Actions
2. The workflow reads the corresponding `meta/*.json` to get the page ID
3. Content is POSTed to WordPress REST API with:
   - `content` field (standard WordPress)
   - `meta._aviaLayoutBuilderCleanData` (Enfold builder data)
   - `meta._aviaLayoutBuilder_active` set to "active"

## Common Tasks

### Edit a Page

```bash
# Make your changes to the content file
vim content/home.txt

# Commit and push
git add content/home.txt
git commit -m "Update home page hero section"
git push origin main
```

### Test Changes on Staging First

```bash
git checkout staging
git pull origin staging

# Make changes
vim content/events.txt

git add content/events.txt
git commit -m "Test: Update events page"
git push origin staging

# After verifying on staging.example.com, merge to main
git checkout main
git merge staging
git push origin main
```

### Pull Fresh Content from WordPress

```bash
export WP_USERNAME='username'
export WP_APP_PASSWORD='app-password'
export WP_BASE_URL='https://example.com'

./scripts/pull-page.sh <page-id> <filename>
```

### Deploy All Pages (Manual)

Use GitHub Actions "Run workflow" with "Deploy all pages" checked.

## Enfold Shortcode Basics

Pages use Enfold's shortcode syntax. Key patterns:

```
[av_section]           - Full-width container section
  [av_one_full first]  - Column (first column needs 'first' attribute)
    [av_heading]       - Heading element
    [av_textblock]     - Text content (can contain HTML)
    [av_button]        - Button element
  [/av_one_full]
[/av_section]
```

## Important Notes

1. **File sizes can be large** - Some pages are 300-600KB. The deploy script handles this.

2. **Always match filenames** - `content/foo.txt` must have `meta/foo.json`

3. **Page IDs are environment-specific** - Production and staging have different page IDs. The meta files contain production IDs.

4. **Cache clearing** - After deployment, SiteGround cache may need clearing.

5. **Shortcodes are sensitive** - Malformed shortcodes can break the page. Test on staging first.

## Credentials

The workflow uses these GitHub Secrets for both environments:

- `USERNAME` - WordPress username
- `APP_PASSWORD` - WordPress application password

SiteGround staging sites share credentials with production.

## Troubleshooting

### Deployment succeeds but changes don't appear
- Clear SiteGround cache (SG Optimizer → Purge Cache)
- Verify the Enfold REST Meta plugin is activated
- Check that the page ID in meta/*.json is correct

### 401 Unauthorized
- Check that GitHub Secrets are set correctly
- Verify the WordPress application password is valid

### Shortcodes render as text
- The Enfold REST Meta plugin may not be activated
- Check that `_aviaLayoutBuilder_active` is set to "active"

## Working with Claude Desktop

This repo is designed for a two-agent workflow:

1. **Claude Desktop** generates Enfold shortcode content and str_replace instructions
2. **Claude Code** (you) executes the edits, commits, and pushes

When Claude Desktop provides edit instructions, they'll typically be in str_replace format:
```
old_str:
[exact text to find]

new_str:
[replacement text]
```

Apply these with care - Enfold shortcodes have many parameters and whitespace matters.
