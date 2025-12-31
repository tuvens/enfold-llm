# Theme Customization Guide

This document explains how to work with the Your Site site's design system and theme settings.

## Quick Reference

**Design Tokens Location:** `theme/design-tokens.json`

When creating new pages or content, load the design tokens to ensure consistent branding:

```bash
cat theme/design-tokens.json
```

## Brand Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `primary` | `#409265` | Links, buttons, CTAs, accents |
| `secondary` | `#0a364d` | Headers, footers, dark sections |
| `accent` | `#179162` | Highlights, hover states |
| `background` | `#ffffff` | Main content areas |
| `background_alt` | `#f8f8f8` | Alternate sections |
| `text` | `#000000` | Body text |
| `text_muted` | `#969696` | Meta info, captions |
| `border` | `#ebebeb` | Dividers, borders |

## Enfold Shortcode Color Classes

Enfold uses predefined color sets. Map your design tokens to these classes:

| Color Set | Background | Text | When to Use |
|-----------|------------|------|-------------|
| `main_color` | White | Black | Default content sections |
| `alternate_color` | Dark blue (#0a364d) | White | Feature sections, CTAs |
| `header_color` | White | Dark blue | Header-like sections |
| `footer_color` | Dark blue | White | Footer-like sections |
| `socket_color` | White | Dark blue | Bottom bar sections |

### Using in Shortcodes

```
[av_section color='main_color' ...]
  Content with default styling
[/av_section]

[av_section color='alternate_color' ...]
  Content on dark background (white text)
[/av_section]
```

## Typography

| Role | Font |
|------|------|
| Headings | Comfortaa (Google Font) |
| Body | Now Alt (custom uploaded font) |

When specifying fonts in shortcodes, the theme defaults apply automatically.

## Creating New Pages

When creating pages, follow this pattern:

1. **Load design tokens** to confirm current brand colors
2. **Use color set classes** (`main_color`, `alternate_color`) rather than hardcoded hex values
3. **Alternate between sections** for visual rhythm (white → dark blue → white)
4. **Use relative asset paths** - they auto-resolve per environment

### Example Page Structure

```
[av_section color='main_color' padding='large']
  [av_one_full]
    [av_heading tag='h1' color='custom' custom_color='#0a364d']
      Page Title
    [/av_heading]
    [av_textblock]
      Introduction paragraph...
    [/av_textblock]
  [/av_one_full]
[/av_section]

[av_section color='alternate_color' padding='large']
  [av_one_half first]
    Feature content on dark background...
  [/av_one_half]
  [av_one_half]
    More feature content...
  [/av_one_half]
[/av_section]
```

## Modifying Theme Settings

**Never edit generated theme files directly.** 

To change site-wide appearance:

1. Edit `theme/design-tokens.json`
2. Commit to staging branch
3. GitHub Actions regenerates and deploys theme settings
4. Verify on staging site
5. Merge to main for production

### What Can Be Changed

| Setting | Location in design-tokens.json |
|---------|-------------------------------|
| Brand colors | `colors.primary`, `colors.secondary`, etc. |
| Fonts | `typography.heading_font`, `typography.body_font` |
| Logo | `assets.logo` |
| Favicon | `assets.favicon` |
| Preloader | `features.page_preloading` |

### What Requires Manual Setup

- Custom font uploads (via WP Admin → Enfold → Import/Export)
- Media library images (upload first, then reference path)
- Menu structure (via WP Admin → Appearance → Menus)

## Color Derivation Logic

When the theme settings are generated, colors are derived from tokens:

```
Header:
  bg = background
  primary = secondary (dark blue for header accents)
  secondary = accent
  color = accent (menu text)
  heading = secondary

Main Content:
  bg = background
  primary = primary (green for links)
  secondary = secondary
  color = text
  heading = secondary

Alternate (Dark) Sections:
  bg = secondary (dark blue background)
  primary = background (white links on dark)
  color = background (white text)
  heading = background

Footer:
  bg = secondary
  primary = primary
  color = background
  heading = background
```

This ensures visual consistency across the site with minimal input.
