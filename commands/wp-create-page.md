# /wp-create-page - Create a new WordPress page

Create a new page with Enfold shortcodes. This command will:
1. Create content file with YAML frontmatter
2. Generate Enfold shortcode content
3. Stage and commit the file
4. Explain next steps for deployment

## Usage

Use this command when you need to create a completely new page for a WordPress/Enfold site.

## Process

You should:
1. Ask for page details (title, slug, content requirements)
2. Create `content/pages/{slug}.txt` with:
   - YAML frontmatter (title, slug, status)
   - Enfold shortcode content using proper structure
3. Commit the file with descriptive message
4. Explain deployment process (push to staging → test → merge to main)

## Example Output

```txt
---
title: About Us
slug: about-us
status: publish
---
[av_section min_height='' padding='large' color='main_color']
[av_one_full first]
[av_heading heading='About Us' tag='h1' style='blockquote modern-quote modern-centered'][/av_heading]
[av_textblock]
<p>Tell your story here.</p>
[/av_textblock]
[/av_one_full]
[/av_section]
```

## Notes

- The page will be auto-created in WordPress when pushed
- Meta file (`meta/pages/{slug}.json`) will be auto-generated
- Always test on staging branch first