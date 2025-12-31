# Git Worktrees for Multi-Agent Development

A guide to using Git worktrees for coordinating multiple Claude Code sessions working on the same project simultaneously.

## What are Git Worktrees?

Git worktrees allow you to have multiple branches checked out simultaneously in different directories. This enables:
- Multiple Claude Code sessions working in parallel
- No conflicts between simultaneous edits
- Clear separation of concerns
- Fast context switching

## Setting Up Worktrees

### Initial Setup (Human does this once)

```bash
# From main repository directory
mkdir Worktrees  # Create container directory

# Create worktrees for different features
git worktree add Worktrees/feature-homepage feature/homepage
git worktree add Worktrees/fix-contact-form fix/contact-form  
git worktree add Worktrees/import-blog-posts import/blog-posts
```

### Directory Structure

```
project/
├── .git/               # Main git directory
├── content/           # Main branch files
├── meta/
├── scripts/
└── Worktrees/         # Parallel work directories
    ├── feature-homepage/      # Complete checkout of feature/homepage
    │   ├── content/
    │   ├── meta/
    │   └── scripts/
    ├── fix-contact-form/      # Complete checkout of fix/contact-form
    │   ├── content/
    │   ├── meta/
    │   └── scripts/
    └── import-blog-posts/     # Complete checkout of import/blog-posts
        ├── content/
        ├── meta/
        └── scripts/
```

## Claude Code Session Rules

### 1. Stay in Your Lane

Each Claude Code session is assigned ONE worktree:

```bash
# You start here:
cd /project/Worktrees/feature-homepage

# NEVER do this:
cd ../fix-contact-form  # ❌ That's another session's workspace
cd ../../              # ❌ That's the main repository
```

### 2. One Worktree = One Task

- **feature-homepage** → Only edit homepage files
- **fix-contact-form** → Only fix contact form issues
- **import-blog-posts** → Only import blog content

### 3. Verify Your Context

Always start sessions by confirming your location:

```bash
pwd
# Output: /project/Worktrees/feature-homepage

git branch --show-current  
# Output: feature/homepage

git status
# Should show only your intended changes
```

## Workflow in a Worktree

### 1. Make Your Changes

```bash
# Edit ONLY files related to your task
vim content/pages/home.txt
```

### 2. Commit Atomically

```bash
# Stage only your files
git add content/pages/home.txt

# Clear commit message with context
git commit -m "feat(home): Update hero section with new tagline"
```

### 3. Push Your Branch

```bash
# Always push just your branch
git push origin HEAD
```

### 4. Create Pull Request

```bash
# Target staging, not main
gh pr create \
  --base staging \
  --title "Update homepage hero section" \
  --body "- Updated tagline to match new brand messaging
- Adjusted button colors for better contrast
- Tested with screen readers"
```

## Communication Between Sessions

### Branch Naming Conventions

Branch names indicate purpose and scope:

```
feature/mobile-navigation     # New mobile nav implementation
fix/search-button-color      # Fixing specific bug
update/about-team-section    # Content update
import/spanish-translations  # Bulk import task
refactor/testimonial-cards   # Code improvement
```

### Commit Message Patterns

Use conventional commits for clarity:

```
feat(home): Add video background to hero
fix(nav): Correct dropdown menu z-index
content(about): Update executive team bios
style(global): Implement new color palette
refactor(forms): Simplify validation logic
```

### PR Descriptions

Communicate changes and testing:

```markdown
## Summary
- Updated homepage hero with new video background
- Adjusted text overlay for better readability
- Added pause button for accessibility

## Testing
- [x] Tested on desktop Chrome/Firefox/Safari
- [x] Tested on mobile iOS/Android
- [x] Verified autoplay is disabled on mobile
- [x] Screen reader announces pause button

## Screenshots
[Include before/after if visual changes]
```

## Handling Conflicts

### Preventing Conflicts

1. **Scope isolation** - Each worktree has a specific purpose
2. **File ownership** - Don't edit files outside your scope
3. **Regular updates** - Pull staging changes frequently
4. **Quick merges** - Don't let PRs age

### Resolving Conflicts

If conflicts arise:

```bash
# Update your branch with latest staging
git fetch origin
git rebase origin/staging

# Resolve conflicts in your editor
# Then continue
git add .
git rebase --continue
git push --force-with-lease origin HEAD
```

## Common Scenarios

### Scenario 1: Three Claude Sessions Building Features

- Session A: Building new pricing page in `Worktrees/feature-pricing`
- Session B: Adding testimonials in `Worktrees/feature-testimonials`  
- Session C: Fixing nav menu in `Worktrees/fix-navigation`

All can work simultaneously without conflicts.

### Scenario 2: Coordinated Campaign Launch

- Session A: Creating landing page in `Worktrees/feature-summer-landing`
- Session B: Updating theme colors in `Worktrees/feature-summer-theme`
- Session C: Writing blog posts in `Worktrees/content-summer-posts`

Merge in order: theme → landing → posts

### Scenario 3: Emergency Fix During Feature Work

- Session A: Working on big feature in `Worktrees/feature-redesign`
- Session B: Emergency fix in `Worktrees/hotfix-payment-bug`

Hotfix can be merged immediately without affecting feature work.

## Best Practices

### DO:
- ✅ Check `pwd` when session starts
- ✅ Verify branch with `git branch --show-current`
- ✅ Make atomic commits with clear messages
- ✅ Push frequently to share progress
- ✅ Create descriptive PRs
- ✅ Stay within your assigned scope

### DON'T:
- ❌ Change directories to other worktrees
- ❌ Edit files outside your task scope
- ❌ Merge other sessions' branches
- ❌ Force push without lease
- ❌ Leave PRs open for long periods
- ❌ Create meta files manually

## Worktree Lifecycle

### Creation (Human)
```bash
git worktree add Worktrees/feature-name feature/name
```

### Active Development (Claude)
```bash
cd Worktrees/feature-name
# ... work ...
git add, commit, push
gh pr create
```

### Completion (Human)
```bash
# After PR is merged
git worktree remove Worktrees/feature-name
```

## Summary

Git worktrees enable:
- **Parallel development** without conflicts
- **Clear ownership** of features/fixes
- **Isolated testing** of changes
- **Faster delivery** through concurrent work
- **Better organization** of complex projects

When used properly, multiple Claude Code sessions can collaborate on a project as effectively as a human development team!