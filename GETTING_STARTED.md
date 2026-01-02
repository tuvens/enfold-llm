# Getting Started with Enfold LLM Plugin

This plugin enables GitOps workflows for WordPress sites using the Enfold theme. Manage your page content and theme settings through version-controlled files that automatically deploy via GitHub Actions.

## Quick Setup (60 seconds)

**First step:** Run `/wp-setup` in Claude Code to initialize your project.

This interactive wizard will:
- Copy template files to your repo
- Create the required directory structure  
- Guide you through configuration
- Generate deployment scripts

## What You'll Need

Before starting:
- WordPress site with Enfold theme installed
- GitHub repository with Actions enabled
- WordPress Application Password (Admin → Users → Application Passwords)

## After Setup

1. **Install WordPress Plugin**: Upload `wordpress/enfold-rest-meta/` to your site
2. **Configure GitHub Secrets**: Add `USERNAME` and `APP_PASSWORD` 
3. **Set GitHub Variables**: Add `PRODUCTION_URL` and `STAGING_URL`
4. **Create Your First Page**: Run `/wp-create-page` to start

## How It Works

```
Local Content → Git Push → GitHub Actions → WordPress Site
```

Edit text files locally, commit changes, and watch them deploy automatically to your WordPress site. Content is version-controlled with full history.

## Next Steps

- Read `CLAUDE.md` for complete workflow documentation
- Check `context/claude/` for detailed guides
- See `docs/` for installation and configuration help

Ready to begin? Run `/wp-setup` now!