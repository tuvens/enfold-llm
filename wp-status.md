# /wp-status

Check deployment status, workflow health, and content synchronization state.

## Usage

```
/wp-status [options]
```

## Options

- `--check-api`: Test WordPress REST API connectivity
- `--recent <n>`: Show last n workflow runs (default: 5)
- `--orphans`: Check for orphaned meta files
- `--validate`: Validate all content files

## What This Command Checks

### 1. Configuration Status

Verify the GitOps setup is complete:

```bash
# Check configuration file exists
if [[ -f ".claude-wp.json" ]]; then
  echo "✅ Configuration: .claude-wp.json"
  cat .claude-wp.json | jq -r '"   WordPress: \(.wordpress_url)"'
  cat .claude-wp.json | jq -r '"   Staging: \(.staging_url // "not configured")"'
else
  echo "❌ Configuration: .claude-wp.json missing"
  echo "   Run /wp-setup to initialize"
fi
```

### 2. Directory Structure

```bash
echo "📁 Directory Structure:"
for dir in content/pages content/posts meta/pages meta/posts scripts .github/workflows; do
  if [[ -d "$dir" ]]; then
    echo "   ✅ $dir"
  else
    echo "   ❌ $dir (missing)"
  fi
done
```

### 3. Content Summary

```bash
echo ""
echo "📊 Content Summary:"
echo "   Pages: $(find content/pages -name '*.txt' 2>/dev/null | wc -l | tr -d ' ')"
echo "   Posts: $(find content/posts -name '*.txt' 2>/dev/null | wc -l | tr -d ' ')"
echo "   Portfolio: $(find content/portfolio -name '*.txt' 2>/dev/null | wc -l | tr -d ' ')"
echo "   Layouts: $(find content/layouts -name '*.txt' 2>/dev/null | wc -l | tr -d ' ')"
echo ""
echo "   Meta files: $(find meta -name '*.json' 2>/dev/null | wc -l | tr -d ' ')"
```

### 4. Deployment Status

Check recent GitHub Actions runs:

```bash
# Using GitHub CLI if available
if command -v gh &> /dev/null; then
  echo ""
  echo "🚀 Recent Deployments:"
  gh run list --workflow=deploy.yml --limit 5 --json status,conclusion,createdAt,headBranch \
    --jq '.[] | "   \(.createdAt | split("T")[0]) \(.headBranch): \(.conclusion // .status)"'
else
  echo ""
  echo "🚀 Check deployments at:"
  echo "   https://github.com/<owner>/<repo>/actions/workflows/deploy.yml"
fi
```

### 5. Orphan Detection

Find meta files without corresponding content (or vice versa):

```bash
echo ""
echo "🔍 Orphan Check:"

# Meta files without content
for meta in meta/pages/*.json; do
  [[ -f "$meta" ]] || continue
  slug=$(basename "$meta" .json)
  if [[ ! -f "content/pages/$slug.txt" ]]; then
    echo "   ⚠️  Orphan meta: $meta (no content file)"
  fi
done

# Content files without meta (new files - expected)
NEW_COUNT=0
for content in content/pages/*.txt; do
  [[ -f "$content" ]] || continue
  slug=$(basename "$content" .txt)
  if [[ ! -f "meta/pages/$slug.json" ]]; then
    NEW_COUNT=$((NEW_COUNT + 1))
  fi
done
if [[ $NEW_COUNT -gt 0 ]]; then
  echo "   ℹ️  $NEW_COUNT content files pending first deployment"
fi
```

### 6. Content Validation

Quick validation of shortcode structure:

```bash
echo ""
echo "✅ Content Validation:"

ERRORS=0
for file in content/pages/*.txt; do
  [[ -f "$file" ]] || continue
  
  # Check for Enfold shortcodes
  if ! grep -q '\[av_' "$file"; then
    echo "   ❌ $file: No Enfold shortcodes found"
    ERRORS=$((ERRORS + 1))
    continue
  fi
  
  # Check for rendered HTML (should not be present)
  if grep -q '<div.*class="av-' "$file"; then
    echo "   ❌ $file: Contains rendered HTML (not shortcodes)"
    ERRORS=$((ERRORS + 1))
    continue
  fi
  
  # Basic balance check
  OPEN=$(grep -c '\[av_section' "$file" || echo 0)
  CLOSE=$(grep -c '\[/av_section\]' "$file" || echo 0)
  if [[ "$OPEN" != "$CLOSE" ]]; then
    echo "   ⚠️  $file: Unbalanced sections ($OPEN open, $CLOSE close)"
  fi
done

if [[ $ERRORS -eq 0 ]]; then
  echo "   All content files valid"
fi
```

### 7. API Connectivity (optional)

Test WordPress REST API access:

```bash
echo ""
echo "🌐 API Connectivity:"

WP_URL=$(cat .claude-wp.json 2>/dev/null | jq -r '.wordpress_url // empty')
if [[ -n "$WP_URL" ]]; then
  # Test public endpoint (no auth required)
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" "$WP_URL/wp-json/wp/v2/")
  if [[ "$STATUS" == "200" ]]; then
    echo "   ✅ REST API accessible: $WP_URL"
  else
    echo "   ❌ REST API returned HTTP $STATUS"
  fi
  
  # Test authenticated endpoint (requires credentials)
  if [[ -n "$WP_USERNAME" && -n "$WP_APP_PASSWORD" ]]; then
    AUTH_STATUS=$(curl -s -o /dev/null -w "%{http_code}" \
      -u "$WP_USERNAME:$WP_APP_PASSWORD" \
      "$WP_URL/wp-json/wp/v2/pages?per_page=1&context=edit")
    if [[ "$AUTH_STATUS" == "200" ]]; then
      echo "   ✅ Authentication working"
    else
      echo "   ❌ Authentication failed (HTTP $AUTH_STATUS)"
    fi
  else
    echo "   ℹ️  Set WP_USERNAME and WP_APP_PASSWORD to test auth"
  fi
else
  echo "   ❌ No WordPress URL configured"
fi
```

### 8. Theme Settings Status

```bash
echo ""
echo "🎨 Theme Settings:"

if [[ -f "theme/design-tokens.json" ]]; then
  echo "   ✅ Design tokens: theme/design-tokens.json"
  PRIMARY=$(cat theme/design-tokens.json | jq -r '.colors.primary // "not set"')
  echo "   Primary color: $PRIMARY"
else
  echo "   ℹ️  No design tokens configured"
fi
```

## Output Format

Full status output:

```
📋 WordPress GitOps Status
========================================

✅ Configuration: .claude-wp.json
   WordPress: https://example.com
   Staging: https://staging.example.com

📁 Directory Structure:
   ✅ content/pages
   ✅ content/posts
   ✅ meta/pages
   ✅ meta/posts
   ✅ scripts
   ✅ .github/workflows

📊 Content Summary:
   Pages: 27
   Posts: 72
   Portfolio: 30
   Layouts: 12
   Meta files: 141

🚀 Recent Deployments:
   2025-01-01 staging: success
   2024-12-31 staging: success
   2024-12-30 main: success

🔍 Orphan Check:
   No orphaned files found

✅ Content Validation:
   All content files valid

🌐 API Connectivity:
   ✅ REST API accessible
   ✅ Authentication working

🎨 Theme Settings:
   ✅ Design tokens configured
   Primary color: #409265

========================================
Status: ✅ Healthy
```

## Error States

| State | Meaning | Fix |
|-------|---------|-----|
| Missing .claude-wp.json | Not initialized | Run `/wp-setup` |
| Missing directories | Incomplete setup | Run `/wp-setup --update` |
| Orphan meta files | Content deleted but meta remains | Delete orphan meta files |
| Unbalanced shortcodes | Malformed content | Fix shortcode structure |
| API 401 error | Bad credentials | Regenerate Application Password |
| API 404 error | Wrong URL or REST disabled | Check WordPress URL and REST API |

## Related Commands

- `/wp-setup` - Initialize or repair setup
- `/wp-pull-page --all` - Resync all pages from WordPress
- `/wp-create-page` - Create new page
