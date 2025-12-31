# GitHub Actions Setup Guide

This guide walks you through configuring your GitHub repository for the Enfold LLM GitOps workflow. Follow these steps to set up secrets, variables, and permissions required for automated deployment.

## Table of Contents

1. [Repository Secrets Setup](#repository-secrets-setup)
2. [Repository Variables Setup](#repository-variables-setup)
3. [Branch Protection (Recommended)](#branch-protection-recommended)
4. [Workflow Permissions](#workflow-permissions)
5. [Environment Setup (Optional)](#environment-setup-optional)
6. [Verifying Configuration](#verifying-configuration)
7. [Troubleshooting](#troubleshooting)

## Repository Secrets Setup

Secrets store sensitive data like passwords that should never be exposed in your repository.

### Navigate to Secrets

1. Go to your GitHub repository
2. Click on **Settings** tab (repository settings, not account settings)
3. In the left sidebar, find **Secrets and variables**
4. Click on **Actions**
5. Click on the **Secrets** tab

### Required Secrets

#### WP_USERNAME

1. Click **New repository secret**
2. Name: `WP_USERNAME`
3. Value: Your WordPress admin username
4. Click **Add secret**

#### WP_APP_PASSWORD

1. Click **New repository secret** again
2. Name: `WP_APP_PASSWORD`
3. Value: Your WordPress application password (without spaces)
4. Click **Add secret**

> **Important**: Application passwords must not contain spaces. If WordPress generates a password with spaces, remove them before adding to GitHub.

### Getting WordPress Application Password

1. Log into WordPress admin
2. Go to **Users** → **Profile** (or **Users** → **All Users** → Edit your user)
3. Scroll to **Application Passwords** section
4. Enter a name (e.g., "GitHub Actions")
5. Click **Add New Application Password**
6. Copy the generated password (remove spaces)
7. Save it as the `WP_APP_PASSWORD` secret

## Repository Variables Setup

Variables store non-sensitive configuration like URLs.

### Navigate to Variables

1. From **Settings** → **Secrets and variables** → **Actions**
2. Click on the **Variables** tab

### Required Variables

#### PRODUCTION_URL

1. Click **New repository variable**
2. Name: `PRODUCTION_URL`
3. Value: Your production site URL (e.g., `https://mysite.com`)
4. Click **Add variable**

#### STAGING_URL

1. Click **New repository variable**
2. Name: `STAGING_URL`
3. Value: Your staging site URL (e.g., `https://staging.mysite.com`)
4. Click **Add variable**

> **Note**: Do not include trailing slashes in URLs.

## Branch Protection (Recommended)

Protecting your main branch prevents accidental deployments to production.

### Enable Branch Protection

1. Go to **Settings** → **Branches**
2. Click **Add rule** under "Branch protection rules"
3. Branch name pattern: `main`
4. Enable these options:
   - ✅ **Require a pull request before merging**
   - ✅ **Require approvals** (optional, but recommended)
   - ✅ **Dismiss stale pull request approvals when new commits are pushed**
   - ✅ **Require status checks to pass before merging** (if you have tests)
5. Click **Create**

### Why Branch Protection Matters

- **Safety**: Prevents direct pushes to production
- **Review**: Ensures changes are reviewed before deployment
- **Testing**: Forces changes through staging first
- **Audit Trail**: Creates a clear history of who approved what

## Workflow Permissions

GitHub Actions needs proper permissions to run.

### Configure Permissions

For enhanced security, it's best practice to grant permissions to workflows on a case-by-case basis.

#### Recommended: Per-Workflow Permissions

The most secure approach is to define permissions inside your workflow file. This ensures a workflow can only perform its intended actions and allows you to keep the repository's default workflow permissions more restrictive.

1. Open your workflow file (e.g., `.github/workflows/deploy.yml`).
2. Add a `permissions` block to the job that needs to write back to the repository.

```yaml
jobs:
  deploy:
    # ...
    permissions:
      contents: write # Required to commit meta files back to the repo
    steps:
      # ...
```

#### Alternative: Repository-Level Permissions

If you prefer a simpler setup, you can grant write permissions to all workflows in the repository. **Note:** This is less secure as it gives more access than may be needed.

1. Go to **Settings** → **Actions** → **General**
2. Scroll to **Workflow permissions**
3. Select: **Read and write permissions**
4. Enable: **Allow GitHub Actions to create and approve pull requests** (optional)
5. Click **Save**

### Understanding Permissions

- **Read**: Allows workflows to check out code
- **Write**: Allows workflows to update deployment status and commit meta files
- **GITHUB_TOKEN**: Automatically provided token with these permissions

## Environment Setup (Optional)

Environments add an extra layer of deployment control.

### Create Environments

1. Go to **Settings** → **Environments**
2. Click **New environment**

#### Staging Environment

1. Name: `staging`
2. Click **Configure environment**
3. Add environment secrets (optional):
   - Override default secrets for staging only
4. No protection rules needed for staging

#### Production Environment

1. Name: `production`
2. Click **Configure environment**
3. Protection rules:
   - ✅ **Required reviewers** (add usernames)
   - ✅ **Prevent self-review**
   - Deployment branches: `main` only
4. Add environment secrets (optional)

### Update Workflow for Environments

If using environments, update `.github/workflows/deploy.yml`:

```yaml
jobs:
  deploy-staging:
    if: github.ref == 'refs/heads/staging'
    environment: staging
    # ... rest of job

  deploy-production:
    if: github.ref == 'refs/heads/main'
    environment: production
    # ... rest of job
```

## Verifying Configuration

### Check Secrets Are Set

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. You should see:
   - Under **Secrets**: `WP_USERNAME`, `WP_APP_PASSWORD` (values hidden)
   - Under **Variables**: `PRODUCTION_URL`, `STAGING_URL` (values visible)

### Test Workflow Run

1. Make a small change to any content file
2. Commit and push to `staging` branch
3. Go to **Actions** tab
4. Watch the workflow run
5. Check for green checkmarks

### Reading Workflow Logs

1. Click on the workflow run
2. Click on the job name
3. Expand each step to see details
4. Look for:
   - ✅ Successful authentication
   - ✅ Files synced message
   - ✅ Deployment complete

## Troubleshooting

### Common Errors and Solutions

#### "Resource not accessible by integration"

**Cause**: Insufficient workflow permissions

**Solution**:

This error indicates the workflow needs permission to write to the repository. The most secure solution is to grant this permission only for the specific job that requires it.

1. Open your `.github/workflows/deploy.yml` file.
2. Add `permissions: { contents: write }` to the relevant job:
   ```yaml
   jobs:
     your-job-name:
       permissions:
         contents: write
       # ... rest of job
   ```
3. Save the file and re-run the workflow.

**Alternative**: You can enable write permissions for all workflows in **Settings** → **Actions** → **General**, but this is less secure.

#### "Secret WP_USERNAME not found"

**Cause**: Secret not created or typo in name

**Solution**:
1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Verify secret exists with exact name `WP_USERNAME`
3. Case matters - ensure uppercase
4. Re-create if needed

#### "401 Unauthorized" from WordPress

**Cause**: Invalid credentials or application password

**Solution**:
1. Verify WordPress username is correct
2. Regenerate application password in WordPress
3. Update `WP_APP_PASSWORD` secret (remove spaces!)
4. Ensure user has administrator role

#### "GitHub Actions workflow not triggering"

**Cause**: Workflow file issues or branch mismatch

**Solution**:
1. Verify `.github/workflows/deploy.yml` exists
2. Check workflow triggers match your branch names
3. Ensure workflow file is valid YAML
4. Check Actions tab for disabled workflows

#### "Permission denied" errors

**Cause**: WordPress REST API restrictions

**Solution**:
1. Ensure Enfold REST Meta plugin is active
2. Check `.htaccess` for REST API blocks
3. Verify user has `edit_pages` capability
4. Test REST API manually with `curl` to verify credentials and API accessibility. A successful request should return a `200 OK` status and JSON data.

   ```bash
   # Replace with your credentials and URL
   curl -i -u "YOUR_WP_USERNAME:YOUR_WP_APP_PASSWORD" https://your-site.com/wp-json/wp/v2/pages
   ```

### Getting Help

If you encounter issues not covered here:

1. Check the [Actions tab](../../actions) for detailed error logs
2. Review the [troubleshooting guide](../context/claude/troubleshooting.md)
3. Ensure all prerequisites are met:
   - WordPress 5.0+
   - Enfold theme active
   - Application passwords enabled
   - REST API accessible

### Security Best Practices

- **Never** commit secrets to the repository
- **Rotate** application passwords periodically
- **Use** environment-specific secrets when possible
- **Restrict** who can approve production deployments
- **Monitor** the Actions tab for unexpected runs