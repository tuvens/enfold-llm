# WordPress Setup Guide for Enfold LLM

This guide covers the WordPress-side setup required for the Enfold LLM GitOps plugin to communicate with your site.

## Table of Contents

1. [Application Password Creation](#application-password-creation)
2. [Installing the REST Meta Plugin](#installing-the-rest-meta-plugin)
3. [Required Permissions](#required-permissions)
4. [Hosting-Specific Notes](#hosting-specific-notes)
5. [Verification Checklist](#verification-checklist)
6. [Troubleshooting](#troubleshooting)

## Application Password Creation

Application Passwords allow secure authentication without exposing your main WordPress password.

### Method 1: WordPress 5.6+ Native Method (Recommended)

1. Log in to WordPress admin
2. Navigate to **Users → Your Profile** (or **Users → All Users** → Edit user)
3. Scroll down to **Application Passwords** section
4. Enter an application name (e.g., "Enfold LLM GitOps")
5. Click **Add New Application Password**
6. **Important**: Copy the generated password immediately - it won't be shown again!

![Application Password Generation](https://wordpress.org/documentation/files/2020/11/application-passwords.png)

### Method 2: Fallback for Disabled Application Passwords

Some hosting providers disable Application Passwords. If you don't see the section:

1. Add this to your `wp-config.php`:
   ```php
   define( 'WP_APPLICATION_PASSWORDS', true );
   ```

2. If still disabled, install the **Application Passwords** plugin:
   - Download from: https://wordpress.org/plugins/application-passwords/
   - Upload via **Plugins → Add New → Upload Plugin**
   - Activate and follow Method 1 above

### Security Considerations

- Each application password is unique and can be revoked individually
- Store passwords securely (use GitHub Secrets, never commit to repository)
- Use descriptive names to identify each password's purpose
- Regularly review and revoke unused passwords

## Installing the REST Meta Plugin

The Enfold REST Meta plugin extends the WordPress REST API to support Enfold theme operations.

### Method 1: Manual Upload (Easiest)

1. Download the plugin from your repository:
   ```bash
   # From your local enfold-llm repository
   cd wordpress/enfold-rest-meta/
   zip -r enfold-rest-meta.zip .
   ```

2. In WordPress admin:
   - Navigate to **Plugins → Add New → Upload Plugin**
   - Choose the `enfold-rest-meta.zip` file
   - Click **Install Now**
   - Click **Activate**

### Method 2: FTP/SFTP Installation

1. Connect to your server via FTP/SFTP
2. Navigate to `/wp-content/plugins/`
3. Upload the entire `enfold-rest-meta` folder
4. In WordPress admin, go to **Plugins**
5. Find **Enfold REST Meta** and click **Activate**

### Method 3: WP-CLI (Advanced)

```bash
# SSH into your server
cd /path/to/wordpress
wp plugin install /path/to/enfold-rest-meta.zip --activate
```

### Verification Steps

After installation, verify the plugin is working:

1. Go to **Plugins** and confirm **Enfold REST Meta** is Active
2. Visit: `https://yoursite.com/wp-json/enfold-gitops/v1/status`
3. You should see a JSON response (may require authentication)

## Required Permissions

### User Role Requirements

- **Recommended**: Administrator role
- **Minimum**: Editor role with custom capabilities

### REST API Permissions Needed

The user account must have these WordPress capabilities:
- `edit_posts` - Required for post content
- `edit_pages` - Required for page content
- `edit_theme_options` - Required for theme settings
- `upload_files` - Required for media operations

### Custom Capability Requirements

For portfolio items and custom post types:
- `edit_portfolio` (if using Enfold portfolio)
- Custom post type capabilities as needed

### Creating a Dedicated User (Recommended)

1. Go to **Users → Add New**
2. Username: `enfold-gitops` (or similar)
3. Email: Your admin email
4. Role: **Administrator**
5. Generate strong password
6. Create application password for this user

## Hosting-Specific Notes

### WP Engine

WP Engine has specific requirements:

1. Application Passwords work by default
2. Clear cache after plugin activation:
   - WP Engine dashboard → Your Site → Clear All Caches
3. Exclude API endpoints from caching:
   ```
   /wp-json/enfold-gitops/*
   /wp-json/wp/v2/*
   ```

### Cloudflare

Ensure REST API endpoints bypass Cloudflare cache:

1. Go to Cloudflare → Caching → Configuration
2. Add Page Rules for:
   - `*yoursite.com/wp-json/*` → Cache Level: Bypass
3. Consider disabling Rocket Loader for `/wp-json/*`

### Security Plugin Conflicts

#### Wordfence

Add REST API to allowlist:
1. Wordfence → Firewall → Allowlisted URLs
2. Add `/wp-json/enfold-gitops/*`
3. Add `/wp-json/wp/v2/*`

#### Sucuri

1. Go to Sucuri → Settings → Allowlist
2. Add API endpoints
3. Add GitHub Actions IP ranges if needed

#### iThemes Security

1. Security → Settings → WordPress Tweaks
2. Ensure "REST API" is not disabled
3. Add user to REST API allowlist if restricted

## Verification Checklist

Run these commands to verify your setup (replace with your credentials):

```bash
# Set your credentials
USERNAME="your-username"
APP_PASSWORD="your-app-password"
SITE_URL="https://yoursite.com"

# Test 1: Basic REST API access
curl -L -u "$USERNAME:$APP_PASSWORD" \
  "$SITE_URL/wp-json/wp/v2/users/me"

# Test 2: Posts endpoint
curl -L -u "$USERNAME:$APP_PASSWORD" \
  "$SITE_URL/wp-json/wp/v2/posts?per_page=1"

# Test 3: Pages endpoint
curl -L -u "$USERNAME:$APP_PASSWORD" \
  "$SITE_URL/wp-json/wp/v2/pages?per_page=1"

# Test 4: Enfold GitOps status
curl -L -u "$USERNAME:$APP_PASSWORD" \
  "$SITE_URL/wp-json/enfold-gitops/v1/status"

# Test 5: Theme settings access
curl -L -u "$USERNAME:$APP_PASSWORD" \
  "$SITE_URL/wp-json/enfold-gitops/v1/theme-settings"
```

### Expected Results

- ✅ HTTP 200 responses
- ✅ JSON data returned
- ✅ No authentication errors
- ✅ Enfold endpoints accessible

## Troubleshooting

### 401 Unauthorized Errors

**Symptoms**: `{"code":"rest_forbidden","message":"Sorry, you are not allowed to do that."}`

**Solutions**:
1. Verify username and password are correct
2. Check password doesn't contain special characters that need URL encoding
3. Ensure Application Passwords are enabled
4. Try with Basic Auth header:
   ```bash
   curl -L -H "Authorization: Basic $(echo -n $USERNAME:$APP_PASSWORD | base64)" \
     "$SITE_URL/wp-json/wp/v2/users/me"
   ```

### 403 Forbidden Errors

**Symptoms**: Server returns 403 status

**Solutions**:
1. Check `.htaccess` for REST API blocks
2. Verify security plugins aren't blocking requests
3. Check hosting firewall settings
4. Ensure user has required permissions

### REST API Disabled Errors

**Symptoms**: `{"code":"rest_disabled","message":"The REST API is disabled on this site."}`

**Solutions**:
1. Check if REST API is disabled in code:
   ```php
   // Remove this from theme/plugin:
   add_filter('rest_enabled', '__return_false');
   ```

2. Install and activate REST API plugin if needed
3. Check hosting provider settings

### SSL/HTTPS Issues

**Symptoms**: SSL certificate errors, connection refused

**Solutions**:
1. Ensure site URL uses HTTPS in WordPress settings
2. For local development, use:
   ```bash
   curl -kL -u "$USERNAME:$APP_PASSWORD" \
     "$SITE_URL/wp-json/wp/v2/posts"
   ```
3. Update certificates if expired
4. Check mixed content issues

### Plugin Not Found

**Symptoms**: Enfold GitOps endpoints return 404

**Solutions**:
1. Verify plugin is activated in WordPress admin
2. Flush permalinks: Settings → Permalinks → Save Changes
3. Clear all caches (hosting, CDN, browser)
4. Check plugin files exist in `/wp-content/plugins/enfold-rest-meta/`

### Common Error Codes

| Code | Meaning | Solution |
|------|---------|----------|
| `rest_no_route` | Endpoint doesn't exist | Check plugin activation, flush permalinks |
| `rest_forbidden` | Permission denied | Check user role and capabilities |
| `rest_invalid_param` | Bad request data | Review API documentation |
| `rest_cookie_invalid_nonce` | CSRF token issue | Use Application Password auth |

### Getting Help

If you're still experiencing issues:

1. Check WordPress debug log:
   ```php
   // In wp-config.php
   define( 'WP_DEBUG', true );
   define( 'WP_DEBUG_LOG', true );
   define( 'WP_DEBUG_DISPLAY', false );
   ```

2. Review server error logs
3. Test with a minimal setup (default theme, no plugins except Enfold)
4. Open an issue with:
   - WordPress version
   - Hosting provider
   - Error messages
   - Steps to reproduce
