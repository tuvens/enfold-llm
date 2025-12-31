# Enfold Theme Settings Analysis - Forró Federation

## File Structure Overview

The Enfold theme settings export file is:
- **Format:** Base64-encoded PHP serialized array
- **Decoded size:** ~598 KB
- **Total settings:** 447 options in `avia` + 17 in `avia_ext`
- **Settings with values:** 229 (51%)
- **Settings with defaults/empty:** 218 (49%)

## Setting Types Distribution

| Type | Count | Description |
|------|-------|-------------|
| select | 203 | Dropdown choices |
| checkbox | 110 | Toggle options |
| colorpicker | 47 | Color values |
| text | 34 | Text inputs |
| textarea | 12 | Multi-line text |
| hidden | 12 | Internal values |
| upload | 11 | Image/file uploads |
| group | 5 | Grouped settings |
| verification_field | 5 | Validation fields |
| file_upload | 4 | File uploads |
| Other | 4 | Misc types |

---

## Extracted Design Tokens

### 🎨 Color Palette

#### Brand Colors (Primary)
| Role | Hex | Usage |
|------|-----|-------|
| Dark Blue | `#0a364d` | Headers, headings, footers |
| Primary Green | `#409265` | Links, accents, primary actions |
| Secondary Green | `#179162` | Header accents |

#### Section Color Sets

**Header Colors:**
```
Background:    #ffffff (white)
Background 2:  #f8f8f8 (light gray)
Primary:       #0a364d (dark blue)
Secondary:     #179162 (green)
Text Color:    #179162 (green)
Meta:          #0a364d (dark blue)
Headings:      #0a364d (dark blue)
Border:        #ebebeb (light border)
```

**Main Content Colors:**
```
Background:    #ffffff (white)
Background 2:  #f8f8f8 (light gray)
Primary:       #409265 (green)
Secondary:     #0a364d (dark blue)
Text Color:    #000000 (black)
Meta:          #0a364d (dark blue)
Headings:      #0a364d (dark blue)
Border:        #ebebeb (light border)
```

**Alternate Section (Dark) Colors:**
```
Background:    #0a364d (dark blue)
Background 2:  #0a364d (dark blue)
Primary:       #ffffff (white)
Secondary:     #ffffff (white)
Text Color:    #ffffff (white)
Meta:          #ebebeb (light gray)
Headings:      #ffffff (white)
Border:        #ebebeb (light border)
```

**Footer Colors:**
```
Background:    #0a364d (dark blue)
Background 2:  #f8f8f8 (light gray)
Primary:       #409265 (green)
Secondary:     #ffffff (white)
Text Color:    #ffffff (white)
Meta:          #969696 (gray)
Headings:      #ffffff (white)
Border:        #ebebeb (light border)
```

**Socket (Bottom Footer) Colors:**
```
Background:    #ffffff (white)
Background 2:  #f8f8f8 (light gray)
Primary:       #409265 (green)
Secondary:     #409265 (green)
Text Color:    #0a364d (dark blue)
Meta:          #969696 (gray)
Headings:      #000000 (black)
Border:        #ebebeb (light border)
```

### 🔤 Typography

| Setting | Value |
|---------|-------|
| Heading Font | `Comfortaa:300,400,700` (Google Font) |
| Body Font | `now-alt-cufonfonts-webfont-custom` (Custom uploaded) |

**Custom Fonts Uploaded:**
- `aristotelica (400)`
- `now alt cufonfonts webfont (100, 300, 400, 500, 700, 900)`

### 📐 Logo & Branding

| Asset | URL |
|-------|-----|
| Main Logo | `https://staging2.forrofederation.com/wp-content/uploads/2024/05/Untitled-300-x-300-px-100-x-100-px-500-x-100-px-1-300x86.png` |
| Favicon | `https://staging2.forrofederation.com/wp-content/uploads/2024/05/Untitled-300-x-300-px-100-x-100-px.png` |
| Preloader Logo | `https://staging2.forrofederation.com/wp-content/uploads/2024/05/Untitled-300-x-300-px-100-x-100-px-500-x-100-px-250-x-71-px.png` |

### ⚙️ Feature Settings

| Feature | Status |
|---------|--------|
| Page Preloading | Enabled |
| Page Transitions | Enabled |
| Lightbox Modal | Enabled |

---

## Questions & Answers

### Q1: Can Claude generate this file deterministically for new sites?

**Confidence: 70%** - Yes, with caveats.

**What's Possible:**
- The file structure is deterministic (PHP serialized array)
- 229 settings have actual values; 218 use theme defaults
- A minimal export (~34KB JSON) of just the customized values is highly feasible

**Challenges:**
1. **Custom font references** - The file references uploaded fonts (`now-alt-cufonfonts-webfont-custom`). These must be uploaded separately to WordPress first.
2. **Image URLs** - Logo/favicon URLs are site-specific and must exist on the target WordPress media library
3. **Page/post references** - Some settings reference specific WordPress page IDs (e.g., `frontpage`, `blogpage`)
4. **Validation** - Enfold validates certain setting combinations

**Recommended Approach:**
1. Create a **design token template** (JSON) with colors, fonts, and feature flags
2. Claude generates the full settings file from template + base theme defaults
3. GitHub Action uploads to WordPress
4. Media assets uploaded separately or via URL references

### Q2: Can WordPress API deploy this from GitHub?

**Confidence: 85%** - Yes, but requires a custom plugin.

**The Problem:**
- Enfold stores settings in `wp_options` table under option name `avia`
- Standard WordPress REST API `/wp-json/wp/v2/settings/` only exposes registered settings
- Enfold's theme options are NOT registered with the REST API by default

**Solution: Custom REST Endpoint Plugin**

Create a plugin that:
1. Exposes Enfold settings via custom REST endpoint
2. Accepts the base64-encoded settings file
3. Validates and imports settings
4. Regenerates Enfold's dynamic CSS

---

## Proposed Plugin: `enfold-settings-api.php`

```php
<?php
/**
 * Plugin Name: Enfold Theme Settings REST API
 * Description: Exposes Enfold theme settings to REST API for GitOps deployment
 * Version: 1.0
 */

add_action('rest_api_init', function() {
    register_rest_route('enfold-gitops/v1', '/settings', [
        [
            'methods' => 'GET',
            'callback' => 'enfold_gitops_get_settings',
            'permission_callback' => function() {
                return current_user_can('manage_options');
            }
        ],
        [
            'methods' => 'POST',
            'callback' => 'enfold_gitops_update_settings',
            'permission_callback' => function() {
                return current_user_can('manage_options');
            }
        ]
    ]);
});

function enfold_gitops_get_settings() {
    $avia = get_option('avia');
    $avia_ext = get_option('avia_ext', []);
    
    if (!$avia) {
        return new WP_Error('no_settings', 'Enfold settings not found', ['status' => 404]);
    }
    
    // Return base64-encoded serialized data (matching export format)
    $data = serialize(['avia' => $avia, 'avia_ext' => $avia_ext]);
    
    return [
        'success' => true,
        'data' => base64_encode($data),
        'settings_count' => is_array($avia) ? count($avia) : 0
    ];
}

function enfold_gitops_update_settings(WP_REST_Request $request) {
    $encoded_data = $request->get_param('settings');
    
    if (empty($encoded_data)) {
        return new WP_Error('missing_settings', 'No settings data provided', ['status' => 400]);
    }
    
    // Decode and unserialize
    $decoded = base64_decode($encoded_data);
    if ($decoded === false) {
        return new WP_Error('decode_error', 'Invalid base64 encoding', ['status' => 400]);
    }
    
    $settings = @unserialize($decoded);
    if ($settings === false) {
        return new WP_Error('unserialize_error', 'Invalid serialized data', ['status' => 400]);
    }
    
    // Validate structure
    if (!isset($settings['avia']) || !is_array($settings['avia'])) {
        return new WP_Error('invalid_structure', 'Missing avia settings array', ['status' => 400]);
    }
    
    // Update options
    update_option('avia', $settings['avia']);
    
    if (isset($settings['avia_ext'])) {
        update_option('avia_ext', $settings['avia_ext']);
    }
    
    // Trigger Enfold's dynamic CSS regeneration
    if (function_exists('avia_backend_safe_string')) {
        delete_transient('avia_dynamic_stylesheet');
        delete_transient('avia_stylesheet_compiled');
    }
    
    // Clear any caches
    if (function_exists('wp_cache_flush')) {
        wp_cache_flush();
    }
    
    return [
        'success' => true,
        'message' => 'Enfold settings updated successfully',
        'settings_count' => count($settings['avia'])
    ];
}
```

---

## GitHub Actions Workflow for Settings Deployment

```yaml
# .github/workflows/deploy-theme-settings.yml
name: Deploy Enfold Theme Settings

on:
  push:
    branches: [main, staging]
    paths:
      - 'theme-settings/enfold-settings.txt'
  workflow_dispatch:

jobs:
  deploy-settings:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Determine environment
        id: env
        run: |
          if [[ "${{ github.ref_name }}" == "staging" ]]; then
            echo "url=https://staging2.forrofederation.com" >> $GITHUB_OUTPUT
          else
            echo "url=https://forrofederation.com" >> $GITHUB_OUTPUT
          fi
      
      - name: Deploy theme settings
        env:
          WP_USERNAME: ${{ secrets.USERNAME }}
          WP_APP_PASSWORD: ${{ secrets.APP_PASSWORD }}
        run: |
          SETTINGS_FILE="theme-settings/enfold-settings.txt"
          
          if [[ ! -f "$SETTINGS_FILE" ]]; then
            echo "Settings file not found"
            exit 1
          fi
          
          # Read the base64 content
          SETTINGS=$(cat "$SETTINGS_FILE")
          
          # Deploy via REST API
          RESPONSE=$(curl -s -w "\n%{http_code}" \
            -X POST "${{ steps.env.outputs.url }}/wp-json/enfold-gitops/v1/settings" \
            --user "${WP_USERNAME}:${WP_APP_PASSWORD}" \
            -H "Content-Type: application/json" \
            -d "{\"settings\": \"$SETTINGS\"}")
          
          HTTP_CODE=$(echo "$RESPONSE" | tail -n1)
          BODY=$(echo "$RESPONSE" | sed '$d')
          
          if [[ "$HTTP_CODE" -ge 200 && "$HTTP_CODE" -lt 300 ]]; then
            echo "✅ Theme settings deployed successfully"
            echo "$BODY" | jq .
          else
            echo "❌ Deployment failed"
            echo "HTTP Status: $HTTP_CODE"
            echo "Response: $BODY"
            exit 1
          fi
```

---

## Design Token Template (for generating new sites)

For creating new sites, use this JSON template:

```json
{
  "brand": {
    "name": "New Site Name",
    "primary_color": "#409265",
    "secondary_color": "#0a364d",
    "accent_color": "#179162"
  },
  "colors": {
    "background": "#ffffff",
    "background_alt": "#f8f8f8",
    "text": "#000000",
    "text_light": "#969696",
    "border": "#ebebeb"
  },
  "typography": {
    "heading_font": "Comfortaa:300,400,700",
    "body_font": "Open Sans:400,600"
  },
  "assets": {
    "logo_url": "https://example.com/logo.png",
    "favicon_url": "https://example.com/favicon.png"
  },
  "features": {
    "preloader": true,
    "page_transitions": true,
    "lightbox": true
  }
}
```

Claude could then:
1. Accept this template
2. Merge with Enfold's base settings structure
3. Generate the full PHP serialized + base64 encoded file
4. Commit to GitHub for deployment

---

## Summary

| Question | Answer | Confidence |
|----------|--------|------------|
| Extract design patterns? | ✅ Yes - 5 color sets, 2 fonts, feature flags | 95% |
| Generate for new sites? | ✅ Yes - from design token template | 70% |
| Too big to generate? | ❌ No - 34KB minimal, 598KB full | 90% |
| Deploy via WP API? | ✅ Yes - requires custom plugin | 85% |

**Next Steps:**
1. Install the `enfold-settings-api.php` plugin on both staging and production
2. Add `theme-settings/` folder to the GitHub repo
3. Store the base64 settings file in Git
4. GitHub Actions will auto-deploy on changes
