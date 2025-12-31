# /wp-theme - Manage Enfold theme settings via design tokens

Update Enfold theme settings using the design tokens approach. This command will:
1. Edit `theme/design-tokens.json` with new values
2. Explain how changes will be deployed
3. Show what Enfold settings will be affected
4. Stage and commit the changes

## Usage

Use this command to update site colors, fonts, branding, or other theme-level settings.

## Process

You should:
1. Read current `theme/design-tokens.json` to understand structure
2. Make requested changes to colors, typography, branding, or features
3. Explain how the change will be processed:
   - `scripts/generate-theme-settings.py` converts JSON to Enfold format
   - GitHub Actions posts to `/wp-json/enfold-gitops/v1/settings`
   - Plugin imports settings and regenerates CSS
4. Commit changes with descriptive message
5. Remind user to test on staging first

## Design Tokens Structure

```json
{
  "brand": {
    "name": "Your Site Name",
    "tagline": "Your tagline"
  },
  "colors": {
    "primary": "#3b82f6",
    "secondary": "#1e293b", 
    "accent": "#10b981",
    "background": "#ffffff",
    "text": "#000000"
  },
  "typography": {
    "heading_font": "Roboto:400,700",
    "body_font": "Open Sans:400,600"
  },
  "assets": {
    "logo": "/path/to/logo.png",
    "favicon": "/path/to/favicon.png"
  }
}
```

## Examples

- Change primary color: `"primary": "#ff6b6b"`
- Update fonts: `"heading_font": "Montserrat:300,400,700"`
- Change site name: `"name": "My New Site"`

## Notes

- Changes affect the entire site appearance
- Automatic cache clearing happens after deployment
- Use relative paths for assets (e.g., `/wp-content/uploads/logo.png`)
- Font formats: Google Fonts (`"FontName:400,700"`) or uploaded (`"fontname-custom"`)