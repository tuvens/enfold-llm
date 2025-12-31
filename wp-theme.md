# /wp-theme

Manage theme settings and design tokens for WordPress/Enfold sites.

## Usage

```
/wp-theme <action> [options]
```

## Actions

| Action | Description |
|--------|-------------|
| `show` | Display current design tokens |
| `edit` | Modify design tokens |
| `deploy` | Force deploy theme settings |
| `pull` | Pull current settings from WordPress |
| `reset` | Reset tokens to defaults |

## Prerequisites

This command requires:
1. `theme/design-tokens.json` file in repo
2. `scripts/generate-theme-settings.py` script
3. WordPress plugin with theme settings endpoint installed

## Actions in Detail

### /wp-theme show

Display current design tokens in a readable format:

```bash
/wp-theme show
```

Output:
```
🎨 Current Design Tokens
========================================

Brand:
  Name: Forró Federation
  Tagline: Connecting forró communities worldwide

Colors:
  Primary:    #409265 ████ (green - links, buttons)
  Secondary:  #0a364d ████ (dark blue - headers, footers)
  Accent:     #179162 ████ (green - highlights)
  Background: #ffffff ████ (white - main content)
  Text:       #000000 ████ (black - body text)

Typography:
  Headings: Comfortaa:300,400,700
  Body:     now-alt-cufonfonts-webfont-custom

Assets:
  Logo:     /wp-content/uploads/2024/05/logo.png
  Favicon:  /wp-content/uploads/2024/05/favicon.png

Features:
  ✅ Page preloading
  ✅ Page transitions
  ✅ Lightbox
```

Implementation:
```bash
cat theme/design-tokens.json | jq -r '
  "Brand:",
  "  Name: \(.brand.name)",
  "  Tagline: \(.brand.tagline)",
  "",
  "Colors:",
  "  Primary:    \(.colors.primary)",
  "  Secondary:  \(.colors.secondary)",
  "  Accent:     \(.colors.accent)",
  "  Background: \(.colors.background)",
  "  Text:       \(.colors.text)",
  "",
  "Typography:",
  "  Headings: \(.typography.heading_font)",
  "  Body:     \(.typography.body_font)"
'
```

---

### /wp-theme edit

Interactive editing of design tokens:

```
/wp-theme edit --color primary
/wp-theme edit --font heading
/wp-theme edit --logo
```

#### Edit Colors

```
/wp-theme edit --color primary

Current primary color: #409265
Enter new color (hex format, e.g., #ff6600): #3b82f6
```

Update using str_replace:
```bash
str_replace in theme/design-tokens.json

old_str:
    "primary": "#409265",

new_str:
    "primary": "#3b82f6",
```

#### Edit Fonts

```
/wp-theme edit --font heading

Current heading font: Comfortaa:300,400,700

Available Google Fonts (examples):
  - Roboto:400,700
  - Open Sans:400,600
  - Montserrat:400,500,700
  - Lato:400,700
  - Poppins:400,500,600

For custom uploaded fonts, use: fontname-custom

Enter new font:
```

#### Edit Multiple Values

```
/wp-theme edit

What would you like to change?
1. Colors (primary, secondary, accent, etc.)
2. Typography (heading font, body font)
3. Assets (logo, favicon, preloader)
4. Features (preloading, transitions, lightbox)

Choice: 1

Which color? primary
Current: #409265
New value: #3b82f6

Updated theme/design-tokens.json
Commit and push to deploy changes.
```

---

### /wp-theme deploy

Force deploy current design tokens to WordPress:

```
/wp-theme deploy [--env staging|production]
```

This is typically automatic when pushing changes, but can be triggered manually:

```bash
# Generate settings
python3 scripts/generate-theme-settings.py \
  theme/design-tokens.json \
  https://staging2.forrofederation.com \
  /tmp/settings.txt

# Deploy via API
curl -X POST "$WP_URL/wp-json/enfold-gitops/v1/settings" \
  -u "$WP_USERNAME:$WP_APP_PASSWORD" \
  -H "Content-Type: application/json" \
  -d "{\"settings\": \"$(cat /tmp/settings.txt)\"}"
```

Output:
```
🚀 Deploying theme settings to STAGING
   Generating settings from design tokens...
   Settings generated (45 properties, 12KB encoded)
   Posting to WordPress REST API...
   ✅ Theme settings deployed successfully
   
   CSS regeneration triggered. Changes should be visible after cache clear.
   
   Site: https://staging2.forrofederation.com
```

---

### /wp-theme pull

Pull current theme settings from WordPress and update local tokens:

```
/wp-theme pull [--env staging|production]
```

⚠️ **Warning:** This overwrites local design tokens with values from WordPress.

```bash
# Fetch current settings
RESPONSE=$(curl -s "$WP_URL/wp-json/enfold-gitops/v1/settings" \
  -u "$WP_USERNAME:$WP_APP_PASSWORD")

# Extract relevant values and update design-tokens.json
echo "$RESPONSE" | jq -r '.data' | base64 -d | php -r '
  $data = unserialize(file_get_contents("php://stdin"));
  $avia = $data["avia"];
  
  $tokens = [
    "colors" => [
      "primary" => $avia["colorset-main_color-primary"]["std"] ?? "",
      "secondary" => $avia["colorset-main_color-secondary"]["std"] ?? "",
      // ... etc
    ]
  ];
  
  echo json_encode($tokens, JSON_PRETTY_PRINT);
'
```

---

### /wp-theme reset

Reset design tokens to plugin defaults:

```
/wp-theme reset [--confirm]
```

⚠️ **Warning:** This overwrites your customizations with default values.

```bash
/wp-theme reset

This will reset ALL design tokens to defaults:
  - Colors: Standard neutral palette
  - Typography: System fonts
  - Assets: Placeholder paths
  - Features: All enabled

Are you sure? (yes/no): yes

Reset complete. Edit theme/design-tokens.json to customize.
```

---

## Color Set Reference

When editing colors, understand how they map to Enfold's color sets:

| Token | Used In |
|-------|---------|
| `primary` | Main content links, buttons, CTAs |
| `secondary` | Headers, footers, headings (dark background) |
| `accent` | Highlights, hover states, menu text |
| `background` | Main content area background |
| `background_alt` | Alternate section backgrounds |
| `text` | Body text color |
| `text_muted` | Meta info, captions, secondary text |
| `border` | Dividers, borders |

### Enfold Color Sets Derivation

```
Header Color Set:
  bg = background (#ffffff)
  primary = secondary (#0a364d) - for dark accents
  color = accent (#179162) - menu text

Main Color Set:
  bg = background (#ffffff)
  primary = primary (#409265) - links
  color = text (#000000) - body text
  heading = secondary (#0a364d)

Alternate Color Set (dark sections):
  bg = secondary (#0a364d) - dark background
  primary = background (#ffffff) - white links on dark
  color = background (#ffffff) - white text
  heading = background (#ffffff)

Footer Color Set:
  bg = secondary (#0a364d)
  color = background (#ffffff)
```

---

## Workflow Integration

Changes to `theme/design-tokens.json` automatically trigger deployment:

1. Edit design tokens locally or via `/wp-theme edit`
2. Commit and push to staging branch
3. GitHub Actions detects change in `theme/design-tokens.json`
4. Runs `scripts/generate-theme-settings.py`
5. Posts to WordPress REST API
6. Enfold regenerates dynamic CSS
7. Changes visible on site (after cache clear)

---

## Common Tasks

### Change the primary brand color

```
/wp-theme edit --color primary

Current: #409265
New: #3b82f6

git add theme/design-tokens.json
git commit -m "Update primary brand color to blue"
git push origin staging
```

### Switch to a different Google Font for headings

```
/wp-theme edit --font heading

Current: Comfortaa:300,400,700
New: Montserrat:400,500,700

# Commit and push...
```

### View what will be deployed

```
/wp-theme show --full

# Shows complete Enfold settings that will be generated
```

---

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| "No design tokens found" | Missing file | Run `/wp-setup` or create manually |
| "Invalid JSON" | Syntax error | Check JSON formatting |
| "API 404" | Endpoint missing | Update WordPress plugin |
| "API 401" | Auth failed | Check credentials |
| "CSS not updating" | Cache | Clear WordPress/hosting cache |

---

## Related Commands

- `/wp-setup` - Initialize theme settings system
- `/wp-status` - Check theme settings status
- `/wp-create-page` - Uses design tokens for page styling
