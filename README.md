# Enfold LLM

GitOps workflow for managing WordPress/Enfold websites through LLM assistants like Claude.

Create, update, and deploy WordPress pages, posts, portfolio items, and theme settings directly from your AI coding assistant—with full version control and automatic deployment via GitHub Actions.

## Quick Start (60 seconds)

### 1. Add plugin as submodule
```bash
git submodule add https://github.com/tuvens/enfold-llm.git .claude/plugins/enfold-llm
```

### 2. Open Claude Code
Claude will detect the plugin automatically.

### 3. Run setup wizard
```bash
/wp-setup
```

### 4. Follow prompts
The wizard will guide you through configuration and provide next steps for GitHub and WordPress setup.

For detailed instructions, see [docs/INSTALLATION.md](docs/INSTALLATION.md).

## Overview

This project enables a complete content management workflow where:

1. **You describe what you want** to your LLM assistant
2. **The assistant creates/edits content files** in your Git repository  
3. **GitHub Actions automatically deploys** changes to WordPress
4. **Everything is version controlled** with full audit trail

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  LLM Assistant  │────▶│   Git Commit    │────▶│  GitHub Actions │────▶│    WordPress    │
│  (Claude, etc)  │     │   & Push        │     │  (Auto-deploy)  │     │   (REST API)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Features

- **Content Management**: Create and update pages, posts, portfolio items, and custom layouts
- **Theme Settings**: Manage brand colors, fonts, and styling via simple JSON design tokens
- **Automatic Deployment**: Push to Git, changes appear on your site automatically
- **Staging Support**: Test on staging before deploying to production
- **Enfold Shortcodes**: Full support for Enfold's Advanced Layout Builder
- **LLM-Optimized**: Slash commands and context files designed for AI assistants

## Compatibility

### Built For
- **Claude Code** (Anthropic) - Primary development and testing
- **Claude.ai Projects** - With MCP GitHub integration

### May Work With (Untested)
- GitHub Copilot Workspace
- Cursor
- Windsurf
- Other LLM coding assistants with Git capabilities

> **Note**: This project was built and tested specifically with Claude. The underlying scripts and GitHub Actions workflow should work with any Git-capable tool, but the slash commands (`/wp-create-page`, etc.) and context files are optimized for Claude Code. Contributions to support other LLMs are welcome!

---

## Prerequisites

### 1. WordPress Site with Enfold Theme

This project requires [Enfold theme](https://themeforest.net/item/enfold-responsive-multipurpose-theme/4519990) by Kriesi. The workflow uses Enfold's Advanced Layout Builder shortcodes.

### 2. GitHub Account

Required for:
- Hosting your content repository
- Running GitHub Actions for automatic deployment
- Storing credentials securely as GitHub Secrets

Any GitHub plan works, including free accounts.

### 3. WordPress Application Password

Application Passwords provide secure API access without exposing your main login credentials.

#### How to Create an Application Password

1. Log into your WordPress admin dashboard
2. Navigate to **Users → Profile**
3. Scroll to the **Application Passwords** section
4. Enter a name: `GitHub Actions Deploy`
5. Click **Add New Application Password**
6. **⚠️ Copy the password immediately** — it's only shown once!

The password format looks like: `xxxx xxxx xxxx xxxx xxxx xxxx`

> **Troubleshooting**: If you don't see the Application Passwords section:
> - Ensure your site uses HTTPS (required)
> - Check that you're running WordPress 5.6 or later
> - Verify no security plugin is blocking the feature
> - Some managed hosts disable this—contact your host if needed

### 4. WordPress Plugin Installation

The **Enfold REST API Meta Support** plugin must be installed on your WordPress site.

#### Option A: WordPress Plugin Directory *(Recommended)*

<!-- TODO: Update when published -->
> 🚧 **Coming Soon**: The plugin will be available at [wordpress.org/plugins/enfold-rest-meta](https://wordpress.org/plugins/enfold-rest-meta/)

#### Option B: Manual Installation

1. Download the `wordpress/enfold-rest-meta/` folder from this repository
2. Upload to your site's `wp-content/plugins/` directory
3. Activate in **WordPress Admin → Plugins**

---

## Installation

### Step 1: Create Your Repository

**Option A: Use as a template**
```bash
# Clone this repository as a starting point
git clone https://github.com/tuvens/enfold-llm.git my-wordpress-site
cd my-wordpress-site
rm -rf .git
git init
```

**Option B: Copy templates to existing repo**
```bash
# Copy just the templates you need
cp -r enfold-llm/templates/* your-existing-repo/
```

### Step 2: Configure GitHub Secrets

In your repository: **Settings → Secrets and variables → Actions → Secrets**

| Secret Name | Value |
|-------------|-------|
| `USERNAME` | Your WordPress admin username |
| `APP_PASSWORD` | The Application Password (spaces removed) |

### Step 3: Configure GitHub Variables

In your repository: **Settings → Secrets and variables → Actions → Variables**

| Variable Name | Value | Required |
|---------------|-------|----------|
| `PRODUCTION_URL` | `https://yoursite.com` | Yes |
| `STAGING_URL` | `https://staging.yoursite.com` | Optional |

### Step 4: Configure Your Site

Edit `.claude-wp.json` in your repository root:

```json
{
  "wordpress_url": "https://yoursite.com",
  "staging_url": "https://staging.yoursite.com",
  "theme": "enfold",
  "content_types": ["pages", "posts", "portfolio"],
  "default_branch": "main",
  "staging_branch": "staging"
}
```

### Step 5: Install the WordPress Plugin

See [Prerequisites](#4-wordpress-plugin-installation) above.

### Step 6: Test the Connection

Push a small change to trigger GitHub Actions and verify everything works:

```bash
echo "Test" >> content/pages/.gitkeep
git add .
git commit -m "Test deployment workflow"
git push origin staging
```

Check **Actions** tab in GitHub to see the workflow run.

---

## Usage

### With Claude Code

```
> Create a new About page with a hero section and team member grid

Claude will:
1. Create content/pages/about.txt with Enfold shortcodes
2. Commit and push to staging branch
3. GitHub Actions deploys to your staging site
```

### Slash Commands

| Command | Description |
|---------|-------------|
| `/wp-create-page` | Create a new WordPress page |
| `/wp-update-page <slug>` | Update an existing page |
| `/wp-pull-page <slug>` | Pull page from WordPress into Git |
| `/wp-setup` | Initialize GitOps in a new repository |
| `/wp-status` | Check deployment health and configuration |
| `/wp-theme` | Manage design tokens and theme settings |

### Branch Workflow

| Branch | Deploys To | Use For |
|--------|------------|---------|
| `staging` | Staging site | Testing changes |
| `main` | Production site | Live content |

**Always test on staging first!**

---

## Project Structure

```
your-site/
├── .github/workflows/
│   └── deploy.yml              # GitHub Actions workflow
├── content/
│   ├── pages/*.txt             # Page content (Enfold shortcodes)
│   ├── posts/*.txt             # Blog posts
│   ├── portfolio/*.txt         # Portfolio items
│   └── layouts/*.txt           # Custom layouts
├── meta/                       # ⚠️ Auto-generated, don't edit!
│   └── pages/*.json            # WordPress IDs and metadata
├── theme/
│   └── design-tokens.json      # Brand colors, fonts, assets
├── scripts/                    # Deployment scripts
├── claude/                     # LLM documentation and context
└── .claude-wp.json             # Site configuration
```

---

## Critical Rules

These rules prevent common issues:

> ⚠️ **Never manually create or edit meta files**  
> Meta files in `meta/` are auto-generated by GitHub Actions after WordPress creates a resource.

> ⚠️ **Always test on staging first**  
> Push to `staging` branch, verify on your staging site, then merge to `main`.

> ⚠️ **Content must be Enfold shortcodes, not HTML**  
> Files should contain `[av_section]...[/av_section]`, not `<div class="av-section">`.

> ⚠️ **New pages require YAML frontmatter**  
> Include `title`, `slug`, and `status` in frontmatter for new pages.

---

## Documentation

| Document | Description |
|----------|-------------|
| [commands/wp-create-page.md](commands/wp-create-page.md) | Creating new pages |
| [commands/wp-update-page.md](commands/wp-update-page.md) | Updating existing pages |
| [commands/wp-pull-page.md](commands/wp-pull-page.md) | Pulling pages from WordPress |
| [context/enfold-shortcodes.md](context/enfold-shortcodes.md) | Enfold shortcode reference |
| [context/theme-customization.md](context/theme-customization.md) | Design tokens guide |
| [context/workflow-rules.md](context/workflow-rules.md) | Critical workflow rules |

---

## WordPress Plugin

The `enfold-rest-meta` plugin provides three capabilities:

### 1. REST API for Enfold Post Types
Enables API access for `portfolio`, `avia_layout_builder`, and `alb_custom_layout`.

### 2. Page Builder Meta Fields
Exposes `_aviaLayoutBuilderCleanData` and `_aviaLayoutBuilder_active` to the REST API.

### 3. Theme Settings API
Import/export Enfold theme settings for GitOps-based theme management.

### REST Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/wp-json/wp/v2/pages` | GET/POST | Required | Pages |
| `/wp-json/wp/v2/posts` | GET/POST | Required | Posts |
| `/wp-json/wp/v2/portfolio` | GET/POST | Required | Portfolio items |
| `/wp-json/wp/v2/alb_custom_layout` | GET/POST | Required | Custom layouts |
| `/wp-json/enfold-gitops/v1/settings` | GET/POST | Admin | Theme settings |

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Areas where help is appreciated:**
- Testing with other LLMs (Copilot, Cursor, etc.)
- Additional Enfold shortcode documentation
- Translations
- Bug fixes and error handling improvements

---

## License

This project is licensed under the **GNU General Public License v2.0 or later** (GPL-2.0-or-later).

This license was chosen for WordPress compatibility—WordPress itself is GPL licensed, and plugins/themes that integrate with WordPress typically use GPL as well.

You are free to:
- Use this project commercially or non-commercially
- Modify and distribute your changes
- Fork and create derivative works

See [LICENSE](LICENSE) for the full license text.

---

## Credits

- Created by [Your Site](https://example.com)
- Built for [Claude](https://claude.ai) by Anthropic
- Designed for [Enfold Theme](https://kriesi.at/themes/enfold/) by Kriesi

---

## Support

- **Bug Reports**: [GitHub Issues](https://github.com/tuvens/enfold-llm/issues)
- **Questions**: [GitHub Discussions](https://github.com/tuvens/enfold-llm/discussions)
- **Security Issues**: See [SECURITY.md](SECURITY.md)

---

*This project is not affiliated with or endorsed by Anthropic, Kriesi, or WordPress. Enfold is a trademark of Kriesi.*

