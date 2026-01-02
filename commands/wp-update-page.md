# /wp-update-page - Update existing WordPress page

Update an existing page by editing its content file. This command will:
1. Identify the correct content file 
2. Make the requested edits using proper Enfold shortcodes
3. Stage and commit changes
4. Explain deployment process

## Usage

Use this command when you need to modify content, layout, or styling of an existing page.

## Process

You should:
1. Identify the page to edit (check `content/pages/` directory)
2. Read current content to understand structure
3. Make requested changes using proper Enfold shortcode syntax
4. Preserve existing structure and styling unless specifically asked to change
5. Commit with descriptive message explaining the changes
6. Remind user to test on staging first

## Important Notes

- Existing pages do NOT have YAML frontmatter (only new pages do)
- Use precise str_replace for edits to avoid breaking shortcode syntax
- Large content files (300-600KB) are normal for complex pages
- Always maintain proper nesting: sections > columns > elements
- First column in a row needs `first` attribute

## Reference Documentation

For complex layouts or unfamiliar shortcodes, consult the full schema:

- **Full Schema:** `context/enfold-knowledge/enfold-schema.json` - Complete parameter reference for all shortcodes
- **Best Practices:** `context/enfold-knowledge/enfold-guidance.md` - Patterns and recommendations
- **Quick Reference:** `context/claude/enfold-shortcodes.md` - Common shortcodes (auto-loaded)

When updating pages with advanced elements (tabs, accordions, maps, sliders), read the relevant section from `enfold-schema.json` first.

## Enfold Structure Reminder

```
[av_section]           # Full-width container
  [av_one_full first]  # Column (first needs 'first')
    [av_heading]       # Content elements
    [av_textblock]
    [av_button]
  [/av_one_full]
[/av_section]
```