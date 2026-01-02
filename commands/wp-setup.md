# /wp-setup - Initialize WordPress/Enfold GitOps repository

**Interactive Setup Wizard** - Initialize a new WordPress/Enfold GitOps project with guided configuration.

> **Tip:** Starting fresh? Use the [vibing-enfold](https://github.com/tuvens/vibing-enfold) template instead - it includes everything pre-configured.

## What This Command Does

This wizard will:
1. **Ask for your site details** (URLs, name, description)
2. **Copy all template files** to your project root
3. **Create directory structure** with proper organization
4. **Generate configuration file** (`.claude-wp.json`)
5. **Provide manual setup instructions** for GitHub and WordPress

## Before You Start

Make sure you have:
- WordPress site with Enfold theme installed
- GitHub repository with Actions enabled
- WordPress Application Password ready

## Setup Process

### Step 1: Project Information
Ask the user for:
- **Production URL** (e.g., https://yoursite.com)
- **Staging URL** (optional, e.g., https://staging.yoursite.com)
- **Site Name** (for documentation)
- **Site Description** (optional, for context)

### Step 2: Automatic File Setup
Copy these template files to project root:
- `.github/workflows/deploy.yml` - GitHub Actions workflow
- `scripts/` directory - Deployment and management scripts
- `theme/design-tokens.json` - Theme customization file
- `claude/` directory - Documentation and guides

### Step 3: Directory Structure
Create content directories with `.gitkeep` files:
- `content/pages/`, `content/posts/`, `content/portfolio/`, `content/layouts/`
- `meta/pages/`, `meta/posts/`, `meta/portfolio/`, `meta/layouts/`

### Step 4: Generate Configuration
Create `.claude-wp.json` in project root with user's settings:
```json
{
  "site_name": "User's Site Name",
  "site_description": "User's description",
  "production_url": "https://usersite.com",
  "staging_url": "https://staging.usersite.com",
  "setup_date": "2025-01-02",
  "version": "1.0.0"
}
```

### Step 5: Manual Configuration Instructions
After the wizard completes, provide clear next steps:

## ✅ Automatic Setup Complete!

Your project structure is ready. Now complete these manual steps:

### 1. Install WordPress Plugin
1. Download the plugin folder: `wordpress/enfold-rest-meta/`
2. Upload to your WordPress site: `wp-content/plugins/enfold-rest-meta/`
3. Activate the plugin in WordPress admin

### 2. Configure GitHub Repository
Add these **Secrets** in GitHub → Settings → Secrets and variables → Actions:
- `USERNAME` - Your WordPress username (with editor permissions)
- `APP_PASSWORD` - WordPress Application Password

Add these **Variables** in GitHub → Settings → Secrets and variables → Actions:
- `PRODUCTION_URL` - {production_url from config}
- `STAGING_URL` - {staging_url from config}

### 3. Test Your Setup
```bash
# Create your first page
/wp-create-page

# Check status
/wp-status

# Push to staging for testing
git add -A && git commit -m "feat: Initial setup"
git push origin staging
```

### 4. Customize Your Site
- Edit `theme/design-tokens.json` for branding
- Add content in `content/pages/`
- Review deployment in GitHub Actions

**Ready to start building!** 🚀

## Troubleshooting

If setup fails:
1. Check that all prerequisite requirements are met
2. Verify GitHub repository has Actions enabled
3. Ensure WordPress has Enfold theme installed
4. See `context/claude/troubleshooting.md` for detailed help
