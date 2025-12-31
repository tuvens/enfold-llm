# Claude Code Setup Instructions

You are setting up the `enfold-llm` repository - a reusable GitOps plugin for WordPress/Enfold sites that works with LLM assistants.

## Your Task

Organize this repository by pulling useful content from two reference repositories, then structure everything properly for distribution as a Claude Code plugin.

## Source Repositories

### 1. Your Site (`../Forro-Federation/`)

**IMPORTANT: Checkout the `staging` branch first** - it has the latest files.

```bash
cd ../Forro-Federation
git fetch origin
git checkout staging
git pull origin staging
```

**Pull these files/directories:**
- `.github/workflows/deploy.yml` → Generalize URLs to use `${{ vars.* }}`
- `scripts/*.sh` and `scripts/*.py` → All deployment scripts
- `wordpress/plugins/enfold-rest-meta/` → The WordPress plugin (already generic)
- `claude/*.md` → Documentation (generalize site-specific references)
- `theme/design-tokens.json` → Structure only, replace FoFed colors with generic defaults
- `CLAUDE.md` → Main instructions (generalize)

**⚠️ DO NOT LOAD OR COPY these directories:**
- `content/pages/*.txt` - Site-specific Enfold shortcode content (100+ large files)
- `content/posts/*.txt` - Site-specific posts
- `content/portfolio/*.txt` - Site-specific portfolio items  
- `content/layouts/*.txt` - Site-specific layouts
- `meta/**/*.json` - Site-specific WordPress IDs

These content files are huge and will overwhelm your context. You only need to know they exist and preserve the directory structure with `.gitkeep` files.

**Extract these conventions:**
- Branching strategy: `staging` for testing, `main` for production
- Git worktree approach for parallel development
- Feature branch workflow: branch off staging → PR → merge
- Never commit directly to main for large changes
- Meta files are auto-generated, never manually created

### 2. Enfold WP Editor (`../enfold-wp-editor/`)

Check if this contains any useful scripts, documentation, or tooling that should be incorporated. This may have earlier versions of some tools.

## Target Structure

Organize the `enfold-llm` repo into this structure:

```
enfold-llm/
├── .github/
│   └── workflows/
│       └── deploy.yml              # Generalized workflow template
├── commands/                        # Slash commands for Claude Code
│   ├── wp-create-page.md
│   ├── wp-update-page.md
│   ├── wp-pull-page.md
│   ├── wp-setup.md
│   ├── wp-status.md
│   └── wp-theme.md
├── context/                         # Auto-loaded context files
│   ├── workflow-rules.md           # Critical rules
│   ├── enfold-shortcodes.md        # Shortcode reference
│   └── theme-customization.md      # Design tokens guide
├── templates/                       # Starter templates for new projects
│   ├── .github/
│   │   └── workflows/
│   │       └── deploy.yml
│   ├── content/
│   │   ├── pages/.gitkeep
│   │   ├── posts/.gitkeep
│   │   ├── portfolio/.gitkeep
│   │   └── layouts/.gitkeep
│   ├── meta/
│   │   ├── pages/.gitkeep
│   │   ├── posts/.gitkeep
│   │   ├── portfolio/.gitkeep
│   │   └── layouts/.gitkeep
│   ├── theme/
│   │   └── design-tokens.json      # Generic default tokens
│   ├── scripts/                    # All deployment scripts
│   ├── claude/                     # Project-specific Claude docs
│   └── .claude-wp.json             # Example config
├── wordpress/
│   └── enfold-rest-meta/           # WordPress plugin
│       ├── enfold-rest-meta.php
│       └── README.md
├── plugin.json                      # Claude Code plugin manifest
├── README.md                        # Already exists
├── CONTRIBUTING.md                  # Already exists
├── SECURITY.md                      # Already exists
├── LICENSE                          # Already exists
└── CLAUDE.md                        # Instructions for using the plugin
```

## Generalization Rules

When copying files, make these changes:

### URLs
```
# Before (site-specific)
https://example.com
https://staging.example.com

# After (variable-based)
${{ vars.PRODUCTION_URL }}
${{ vars.STAGING_URL }}
```

### Design Tokens
Replace FoFed brand colors with generic defaults:
```json
{
  "colors": {
    "primary": "#3b82f6",
    "secondary": "#1e293b",
    "accent": "#10b981",
    "background": "#ffffff",
    "text": "#000000"
  }
}
```

### Documentation
- Replace "Your Site" with "your site" or generic examples
- Replace specific URLs with `https://example.com`
- Keep the structure and instructions intact

## Files to Create

If these don't exist in source repos, create them:

1. **`plugin.json`** - Claude Code plugin manifest
2. **`commands/wp-status.md`** - Health check command
3. **`commands/wp-theme.md`** - Theme settings command
4. **`CLAUDE.md`** - Main plugin usage instructions

## Validation Checklist

Before finishing, verify:

- [ ] All scripts are executable (`chmod +x`)
- [ ] No FoFed-specific URLs remain (search for "forrofederation")
- [ ] No site-specific content files were copied
- [ ] Directory structure matches target above
- [ ] `plugin.json` is valid JSON
- [ ] `.gitkeep` files exist in empty directories
- [ ] README links are valid (no broken references)

## Commit Strategy

Make commits in logical groups:
1. "Add deployment workflow and scripts"
2. "Add WordPress plugin"
3. "Add slash commands"
4. "Add context files"
5. "Add templates for new projects"
6. "Add plugin manifest and main docs"

Push to `main` branch when complete.
