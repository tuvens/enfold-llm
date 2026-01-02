# Template Repository Setup Guide

Multiple ways to start a new project with the Enfold LLM plugin, depending on your preference and setup.

## Option 1: GitHub Template Repository (Recommended)

**Best for:** Teams, multiple projects, clean start

If you have a template repository set up:

1. **Use GitHub Template**
   - Click "Use this template" on GitHub
   - Create new repository from template
   - Clone your new repository locally

2. **Initialize Project**
   ```bash
   cd your-new-repo
   # Claude Code should detect the plugin automatically
   /wp-setup
   ```

3. **Benefits**
   - Clean git history (no plugin development commits)
   - All team members get same starting point
   - Easy to create multiple sites

## Option 2: Fork/Clone Existing Repository

**Best for:** Contributing to plugin, single project

1. **Fork or Clone**
   ```bash
   git clone https://github.com/tuvens/enfold-llm.git your-site
   cd your-site
   ```

2. **Remove Plugin Development History** (optional)
   ```bash
   rm -rf .git
   git init
   git add -A
   git commit -m "Initial site setup"
   ```

3. **Initialize Project**
   ```bash
   /wp-setup
   ```

## Option 3: Add as Git Submodule

**Best for:** Existing projects, keeping plugin separate

1. **Add Submodule**
   ```bash
   cd your-existing-repo
   git submodule add https://github.com/tuvens/enfold-llm.git .claude/plugins/enfold-llm
   cd .claude/plugins/enfold-llm
   ```

2. **Initialize Project**
   ```bash
   /wp-setup
   ```

3. **Update Submodule** (for plugin updates)
   ```bash
   git submodule update --remote .claude/plugins/enfold-llm
   ```

## Creating Your Own Template Repository

**For organizations:** Create a template repository for your team.

1. **Fork This Repository**
   - Fork `tuvens/enfold-llm` to your organization
   - Run `/wp-setup` to initialize
   - Customize `theme/design-tokens.json` with your branding
   - Add any standard content in `content/`

2. **Make It a Template**
   - Go to GitHub repository Settings
   - Check "Template repository"
   - Add description: "WordPress/Enfold GitOps template"

3. **Team Usage**
   ```bash
   # Team members create new sites:
   # 1. Use template on GitHub
   # 2. Clone locally
   # 3. Run /wp-setup with site-specific details
   ```

## Comparison

| Method | Git History | Plugin Updates | Team Use | Complexity |
|--------|-------------|----------------|----------|-----------|
| Template | Clean | Manual | ✅ Best | Easy |
| Fork/Clone | Full | Manual | ⚠️ Complex | Medium |
| Submodule | Separate | Automatic | ✅ Good | Hard |

## Recommendations

- **Single site:** Use template or fork
- **Multiple sites:** Create organization template
- **Plugin development:** Fork and contribute back
- **Existing project:** Add as submodule

## Next Steps

Regardless of setup method:
1. Run `/wp-setup` to configure your project
2. Install WordPress plugin from `wordpress/enfold-rest-meta/`
3. Configure GitHub Secrets and Variables
4. Create your first page with `/wp-create-page`

See `GETTING_STARTED.md` for detailed setup instructions.
