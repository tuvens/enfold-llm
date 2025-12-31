# Enfold LLM - GitOps Plugin for WordPress/Enfold Sites

This Claude Code plugin enables GitOps workflows for WordPress sites using the Enfold theme. Manage page content and theme settings through version-controlled files deployed via GitHub Actions.

## Quick Start Commands

| Command | Purpose |
|---------|---------|
| `/wp-setup` | Initialize new project with plugin structure |
| `/wp-create-page` | Create a new page with Enfold shortcodes |
| `/wp-update-page` | Edit existing page content |
| `/wp-pull-page` | Sync content from WordPress to local files |
| `/wp-theme` | Update theme settings via design tokens |
| `/wp-status` | Check GitOps setup health |

## How It Works

```
Local Files ──git push──▶ GitHub Actions ──REST API──▶ WordPress
     │                           │                         │
content/*.txt                   │                    Live Pages
theme/design-tokens.json        │                Theme Settings
     │                           │                         │
     └─────────── Auto-generated meta/*.json ──────────────┘
```

## Key Features

- **Version Control**: All content in git with full history
- **Staging Workflow**: Test changes on staging before production  
- **Design Tokens**: Theme settings as simple JSON
- **Auto Meta**: WordPress IDs auto-generated for new content
- **LLM Optimized**: Designed for AI assistant workflows

## Project Structure

After running `/wp-setup`:

```
your-site/
├── .github/workflows/deploy.yml    # Deployment automation
├── content/
│   ├── pages/*.txt                # Page content (Enfold shortcodes) 
│   ├── posts/*.txt                # Blog posts
│   └── portfolio/*.txt            # Portfolio items
├── meta/
│   └── pages/*.json               # WordPress page IDs (auto-generated)
├── scripts/                       # Deployment scripts
├── theme/design-tokens.json       # Theme settings
└── claude/                        # Documentation
```

## Deployment Flow

1. **Edit** content files or design tokens
2. **Commit** and push to `staging` branch  
3. **GitHub Actions** deploys to staging site
4. **Test** changes on staging environment
5. **Merge** to `main` branch for production deployment

## Requirements

- WordPress 5.0+ with Enfold theme
- GitHub repository with Actions enabled
- WordPress Application Password
- Enfold REST Meta plugin (included)

## Installation

1. Run `/wp-setup` to copy template files
2. Install `wordpress/enfold-rest-meta/` plugin in WordPress
3. Configure GitHub Secrets: `USERNAME`, `APP_PASSWORD`  
4. Set GitHub Variables: `PRODUCTION_URL`, `STAGING_URL`

## Documentation

Complete documentation available in `context/claude/`:
- Creating and updating pages
- Enfold shortcode reference  
- Theme customization
- Troubleshooting guide

## Support

This plugin is designed for WordPress/Enfold sites with GitOps deployment. For issues or questions, refer to the troubleshooting guide in `context/claude/troubleshooting.md`.