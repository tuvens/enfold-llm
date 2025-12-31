# Troubleshooting Guide

Common issues and solutions for the Your Site GitOps workflow.

## Deployment Issues

### GitHub Actions workflow not triggering

**Symptoms:** Push to staging/main doesn't start the workflow

**Causes & Solutions:**

1. **No content changes:** Workflow only triggers on changes to `content/**`
   ```bash
   # Check what files changed
   git diff --name-only HEAD~1 HEAD
   ```

2. **Wrong branch:** Ensure you pushed to `staging` or `main`
   ```bash
   git branch  # Check current branch
   git push origin staging  # Push to correct branch
   ```

3. **Commit message has `[skip ci]`:** Remove this from commit message

### Workflow runs but no files deployed

**Symptoms:** Actions log shows "No content files changed"

**Cause:** The `git diff HEAD~1 HEAD` only checks the most recent commit

**Solution:** For manually re-running, use "Deploy all" option:
1. Go to Actions → Deploy to WordPress → Run workflow
2. Check "Deploy all pages"

### Permission denied when auto-committing meta files

**Symptoms:**
```
remote: Write access to repository not granted.
fatal: unable to access '...': The requested URL returned error: 403
```

**Solution:** Ensure workflow has write permissions. Check `.github/workflows/deploy.yml` has:
```yaml
jobs:
  deploy:
    permissions:
      contents: write
```

### 401 Unauthorized from WordPress API

**Symptoms:** Deployment fails with authentication error

**Solutions:**

1. **Verify GitHub Secrets:**
   - Go to repo Settings → Secrets → Actions
   - Check `USERNAME` and `APP_PASSWORD` are set

2. **Regenerate WordPress App Password:**
   - WordPress Admin → Users → Profile → Application Passwords
   - Create new password, update GitHub Secret

3. **Check user permissions:**
   - User must have edit_posts capability

## Page Creation Issues

### "Missing required field: title"

**Cause:** Content file doesn't have valid YAML frontmatter

**Solution:** Ensure file starts with:
```txt
---
title: Your Page Title
slug: your-page-slug
---
```

### Page created as draft instead of published

**Cause:** Missing `status` field (older workflow defaulted to draft)

**Solution:** Add status to frontmatter:
```txt
---
title: Your Page Title
slug: your-page-slug
status: publish
---
```

### Unknown content type error

**Cause:** File not in recognized folder

**Valid paths:**
- `content/pages/` → Pages
- `content/posts/` → Posts
- `content/portfolio/` → Portfolio
- `content/layouts/` → Enfold Layouts

## Page Update Issues

### Changes pushed but page unchanged

**Checklist:**

1. **Clear SiteGround cache:**
   - SG Optimizer → Purge Cache
   - Or wait ~5 minutes for auto-purge

2. **Verify meta file exists:**
   ```bash
   cat meta/pages/<page-name>.json
   ```

3. **Check page ID is correct:**
   - Compare meta file `post_id` with actual WP page
   - WordPress Admin → Pages → hover over page to see ID

4. **Check Actions log:**
   - Look for errors in deployment step

### Shortcodes render as plain text

**Cause:** Enfold REST Meta plugin not active or meta fields not set

**Solutions:**

1. **Activate plugin:**
   - WordPress Admin → Plugins
   - Ensure "Enfold REST API Meta Support" is active
   - Plugin source: [`wordpress/plugins/enfold-rest-meta/`](../wordpress/plugins/enfold-rest-meta/)

2. **Install plugin if missing:**
   - Copy `wordpress/plugins/enfold-rest-meta/` from this repo to `wp-content/plugins/`
   - Activate in WordPress Admin

3. **Verify deployment sets meta:**
   - Check Actions log shows both `content` and `_aviaLayoutBuilderCleanData`

4. **Manual fix:**
   - Edit page in WordPress
   - Switch to "Advanced Layout Builder"
   - Update/save to refresh builder state

### Page layout broken

**Common causes:**

1. **Unclosed shortcodes:**
   ```
   [av_section]     ← opened
   [av_one_full first]
   ...
   [/av_one_full]
   [/av_section]    ← must close
   ```

2. **Missing `first` attribute:**
   ```
   [av_one_half first]  ← first column needs 'first'
   [av_one_half]        ← subsequent columns don't
   ```

3. **Unbalanced columns:**
   - `av_one_half` + `av_one_half` = full width ✓
   - `av_one_third` + `av_one_half` = broken ✗

## WordPress Issues

### REST API returns 404

**For custom post types (portfolio, layouts):**

1. Verify post type has `show_in_rest => true`
2. Flush permalinks: Settings → Permalinks → Save

### Media files not displaying

**Cause:** Using absolute URLs that don't exist on target environment

**Solution:** Use full URLs from production:
```
src='https://example.com/wp-content/uploads/image.jpg'
```

### Enfold layout not loading

**Cause:** Missing Enfold meta fields

**Verify in database or API:**
- `_aviaLayoutBuilder_active` should be `active`
- `_aviaLayoutBuilderCleanData` should contain shortcodes

### Enfold REST Meta plugin not working

**Symptoms:** API returns 200 but meta fields not updated

**Solutions:**

1. **Verify plugin is active:**
   - WordPress Admin → Plugins
   - Look for "Enfold REST API Meta Support"

2. **Check plugin version:**
   - The plugin in this repo supports: pages, posts, portfolio, layouts
   - Older versions may only support pages

3. **Reinstall from repo:**
   ```bash
   # Plugin source is in this repo
   cat wordpress/plugins/enfold-rest-meta/enfold-rest-meta.php
   ```

## Git Issues

### Merge conflicts in content files

**Solution:**

1. Open the conflicted file
2. Keep the version you want (usually the incoming changes)
3. Remove conflict markers (`<<<<`, `====`, `>>>>`)
4. Test shortcode validity before committing

### Can't push to main (protected branch)

**Solution:** Work through staging:
```bash
git checkout staging
# Make changes
git push origin staging
# After verification
git checkout main
git merge staging
git push origin main
```

## Getting Help

### Debug Information to Gather

1. **GitHub Actions log:** Full output from failed workflow
2. **Git diff:** `git diff HEAD~1 HEAD`
3. **Meta file contents:** `cat meta/pages/<n>.json`
4. **WordPress page ID:** From WP Admin
5. **Content file first 20 lines:** `head -20 content/pages/<n>.txt`

### Useful Commands

```bash
# Check recent workflow runs
gh run list --workflow=deploy.yml

# View specific run
gh run view <run-id>

# Manually trigger deployment
gh workflow run deploy.yml --ref staging

# List all content files
find content -name "*.txt" -type f

# Check for files without meta
for f in content/pages/*.txt; do
  name=$(basename "$f" .txt)
  if [[ ! -f "meta/pages/$name.json" ]] && [[ ! -f "meta/$name.json" ]]; then
    echo "Missing meta: $f"
  fi
done
```

## Required WordPress Setup

For the GitOps workflow to function, WordPress needs:

1. **Enfold REST Meta plugin** - Source: [`wordpress/plugins/enfold-rest-meta/`](../wordpress/plugins/enfold-rest-meta/)
2. **Application Password** - For API authentication
3. **User with edit_posts capability** - To update content via API

See [wordpress/plugins/enfold-rest-meta/README.md](../wordpress/plugins/enfold-rest-meta/README.md) for plugin installation details.
