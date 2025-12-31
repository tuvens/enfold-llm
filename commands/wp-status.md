# /wp-status - Check WordPress/Enfold GitOps health

Check the current status of the GitOps setup and identify any issues. This command will:
1. Verify repository structure
2. Check GitHub workflow configuration
3. Test WordPress connectivity 
4. Validate plugin installation
5. Report any problems found

## Usage

Use this command to diagnose issues or verify everything is working correctly.

## Process

You should:
1. Check required directories exist:
   - `content/pages/`, `content/posts/`, `content/portfolio/`, `content/layouts/`
   - `meta/pages/`, `meta/posts/`, `meta/portfolio/`, `meta/layouts/`
   - `scripts/`, `.github/workflows/`
2. Verify workflow file has correct structure
3. Check for required environment variables
4. Test WordPress REST API connectivity if credentials provided
5. Validate plugin.json format if it exists
6. List recent workflow runs if possible
7. Check for common issues:
   - Missing .gitkeep files in empty directories
   - Mismatched content/meta files
   - Incorrect script permissions
   - Broken shortcode syntax in content files

## Example Output

```
✅ Repository structure valid
✅ GitHub workflow configured  
✅ WordPress plugin detected
⚠️  Missing environment variables
❌ WordPress REST API connection failed

Issues found:
- Set WP_USERNAME and WP_APP_PASSWORD environment variables
- Check WordPress Application Password is valid
- Verify Enfold REST Meta plugin is activated

Recent files:
- content/pages/home.txt (2 hours ago)
- meta/pages/about.json (1 day ago)
```

## Troubleshooting

The command should check for and explain:
- 401 Unauthorized → Check GitHub Secrets
- Plugin not found → Install enfold-rest-meta plugin
- Content/meta mismatch → Run deployment to auto-generate meta
- Shortcodes as text → Activate plugin and check meta fields