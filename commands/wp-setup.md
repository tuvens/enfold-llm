# /wp-setup - Initialize WordPress/Enfold GitOps repository

Set up a new project with the Enfold LLM plugin structure. This command will:
1. Copy template files to create project structure
2. Install WordPress plugin
3. Configure GitHub repository settings
4. Set up deployment workflow

## Usage

Use this command when starting a new WordPress/Enfold GitOps project.

## Process

You should:
1. Copy template files from `templates/` to project root:
   - `.github/workflows/deploy.yml`
   - `scripts/` directory
   - `theme/design-tokens.json`
   - `claude/` documentation
2. Create empty directories with `.gitkeep` files:
   - `content/pages/`
   - `content/posts/`
   - `content/portfolio/`
   - `content/layouts/`
   - `meta/pages/`
   - `meta/posts/`
   - `meta/portfolio/`
   - `meta/layouts/`
3. Install WordPress plugin: copy `wordpress/enfold-rest-meta/` to `wp-content/plugins/`
4. Configure GitHub repository:
   - Set Secrets: `USERNAME`, `APP_PASSWORD`
   - Set Variables: `PRODUCTION_URL`, `STAGING_URL`

## Required Configuration

### GitHub Secrets
- `USERNAME` - WordPress username with editor permissions
- `APP_PASSWORD` - WordPress Application Password

### GitHub Variables  
- `PRODUCTION_URL` - Production site URL (e.g., https://yoursite.com)
- `STAGING_URL` - Staging site URL (e.g., https://staging.yoursite.com)

### WordPress Plugin
Upload and activate the Enfold REST Meta plugin from `wordpress/enfold-rest-meta/`

## Next Steps

1. Customize `theme/design-tokens.json` with your branding
2. Create your first page with `/wp-create-page`
3. Test deployment on staging branch
4. Enable production deployment by removing workflow blocker