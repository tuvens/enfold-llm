# Installation Guide for Enfold LLM Plugin

This guide will help you integrate the Enfold LLM GitOps plugin into your existing WordPress/Enfold project.

## Prerequisites

Before starting, ensure you have:

- **WordPress 5.0+** with Enfold theme installed and activated
- **GitHub repository** with Actions enabled for your WordPress project
- **Claude Code** or compatible LLM assistant
- **Git** installed on your local development machine
- **GitHub CLI (gh)** installed for easier workflow management

## Installation Methods

Choose one of the following methods based on your needs:

### Method A: Git Submodule (Recommended)

**Best for:** Projects that want to receive updates to the plugin

```bash
# Add enfold-llm as a submodule
git submodule add https://github.com/tuvens/enfold-llm.git .claude/plugins/enfold-llm

# Initialize and update the submodule
git submodule update --init --recursive

# Commit the submodule addition
git add .gitmodules .claude/plugins/enfold-llm
git commit -m "feat: Add enfold-llm plugin as submodule"
```

To update the plugin later:
```bash
cd .claude/plugins/enfold-llm
git pull origin main
cd ../../../
git add .claude/plugins/enfold-llm
git commit -m "chore: Update enfold-llm plugin"
```

### Method B: Direct Copy

**Best for:** Simple setup without need for updates

```bash
# Clone the plugin repository temporarily
git clone https://github.com/tuvens/enfold-llm.git /tmp/enfold-llm

# Copy necessary files to your project
cp -r /tmp/enfold-llm/templates/* .
cp -r /tmp/enfold-llm/commands .claude/
cp -r /tmp/enfold-llm/context .claude/
cp /tmp/enfold-llm/plugin.json .claude/
cp -r /tmp/enfold-llm/wordpress .claude/

# Clean up
rm -rf /tmp/enfold-llm

# Commit the files
git add .
git commit -m "feat: Add enfold-llm plugin files"
```

## WordPress Setup

### 1. Install the REST Meta Plugin

Upload the Enfold REST Meta plugin to your WordPress installation:

```bash
# From your project root
cd .claude/plugins/enfold-llm/wordpress/enfold-rest-meta/

# ZIP the plugin
zip -r enfold-rest-meta.zip enfold-rest-meta.php README.md

# Upload to WordPress via admin panel:
# 1. Go to Plugins → Add New → Upload Plugin
# 2. Choose enfold-rest-meta.zip
# 3. Install and Activate
```

### 2. Create Application Password

1. Log in to WordPress admin panel
2. Go to **Users → Your Profile**
3. Scroll to **Application Passwords** section
4. Enter a name (e.g., "GitHub Actions")
5. Click **Add New Application Password**
6. **Save the password** - you'll need it for GitHub configuration

### 3. Verify REST API Access

Test the REST API is working:

```bash
# Replace with your WordPress URL and credentials
curl -u "username:app-password" https://your-site.com/wp-json/wp/v2/pages
```

## GitHub Configuration

### 1. Add Repository Secrets

In your GitHub repository:

1. Go to **Settings → Secrets and variables → Actions**
2. Add these secrets:
   - `WP_USERNAME`: Your WordPress username
   - `WP_APP_PASSWORD`: The application password you created

### 2. Add Repository Variables

In the same settings area, add these variables:
- `PRODUCTION_URL`: Your production WordPress URL (e.g., `https://your-site.com`)
- `STAGING_URL`: Your staging WordPress URL (e.g., `https://staging.your-site.com`)

### 3. Enable GitHub Actions

1. Go to **Actions** tab in your repository
2. If prompted, enable Actions for the repository

## Project Initialization

### 1. Run the Setup Command

With Claude Code, run:

```
/wp-setup
```

This will:
- Copy workflow files to `.github/workflows/`
- Create directory structure for content
- Set up theme configuration files

### 2. Configure Project Settings

Create `.claude-wp.json` in your project root:

```json
{
  "site_url": "https://your-site.com",
  "staging_url": "https://staging.your-site.com",
  "default_branch": "main",
  "staging_branch": "staging"
}
```

### 3. Verify Setup

Run the status command to check everything is configured:

```
/wp-status
```

Expected output:
- ✅ Plugin files found
- ✅ GitHub workflows configured
- ✅ Directory structure ready
- ✅ WordPress plugin active

## First Deployment Test

### 1. Create a Test Page

```
/wp-create-page
```

When prompted, create a simple test page with:
- Title: "GitOps Test Page"
- Slug: "gitops-test"
- Content: Basic text content

### 2. Deploy to Staging

```bash
# Commit the new page
git add content/pages/gitops-test.txt
git commit -m "feat: Add GitOps test page"

# Push to staging branch
git push origin staging
```

### 3. Monitor Deployment

1. Go to **Actions** tab in GitHub
2. Watch the "Deploy to Staging" workflow
3. Check logs for any errors

### 4. Verify on WordPress

Visit `https://staging.your-site.com/gitops-test` to see your deployed page.

## Troubleshooting

### Common Issues

#### "Authentication Failed" in GitHub Actions

**Problem:** The workflow fails with authentication error
**Solution:** 
- Verify `WP_USERNAME` and `WP_APP_PASSWORD` secrets are set correctly
- Ensure application password doesn't contain spaces
- Check WordPress user has appropriate permissions

#### "Plugin Not Found" Error

**Problem:** WordPress REST API returns 404 for Enfold endpoints
**Solution:**
- Verify Enfold REST Meta plugin is activated
- Clear WordPress cache
- Check plugin file permissions

#### "Directory Not Found" During Setup

**Problem:** `/wp-setup` fails to create directories
**Solution:**
- Ensure you're running from project root
- Check file system permissions
- Manually create missing directories if needed

#### GitHub Actions Not Triggering

**Problem:** Pushes don't trigger deployment workflows
**Solution:**
- Check Actions are enabled in repository settings
- Verify workflow files are in `.github/workflows/`
- Ensure branch names match workflow triggers

### Getting Help

1. Check existing documentation in `.claude/context/`
2. Review [troubleshooting guide](../context/claude/troubleshooting.md)
3. Open an issue on [enfold-llm repository](https://github.com/tuvens/enfold-llm/issues)

## Next Steps

After successful installation:

1. **Create your first real page**: `/wp-create-page`
2. **Sync existing content**: `/wp-pull-page`
3. **Customize theme**: Edit `theme/design-tokens.json`
4. **Set up team workflow**: See [workflow rules](../context/workflow-rules.md)

Remember the golden rules:
- Always test on staging first
- Use feature branches for development
- Let GitHub Actions handle deployments
- Never manually edit meta files