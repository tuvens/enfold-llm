# /wp-pull-page - Pull page content from WordPress

Retrieve current page content from WordPress to sync with local files. This command will:
1. Use the pull-page script to fetch content from WordPress
2. Update the local content file
3. Explain any differences found
4. Stage changes for commit

## Usage

Use when you need to sync local files with changes made directly in WordPress admin.

## Process

You should:
1. Set up environment variables (WP_USERNAME, WP_APP_PASSWORD, WP_BASE_URL)
2. Run the pull-page script: `./scripts/pull-page.sh <page-id> <filename>`
3. Review changes and stage if appropriate
4. Explain any significant differences between local and remote content

## Example Commands

```bash
# Set credentials
export WP_USERNAME='your-username'
export WP_APP_PASSWORD='your-app-password'  
export WP_BASE_URL='https://example.com'

# Pull specific page (find page ID from meta file or WordPress admin)
./scripts/pull-page.sh 123 home

# Pull all pages for comparison
./scripts/list-pages.sh
```

## Notes

- Page IDs can be found in `meta/pages/{name}.json` files
- Environment-specific: production and staging have different page IDs
- Use staging URL when testing: `https://staging.example.com`
- Backup local changes before pulling to avoid overwrites