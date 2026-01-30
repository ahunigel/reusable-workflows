# GitHub Configuration Guide

This repository uses the following GitHub features:

## Dependabot

Configuration file: `.github/dependabot.yml`

### Features
- Automatically checks for GitHub Actions updates every Monday at 9 AM
- Automatically creates PRs to update dependencies
- Supports grouped updates (minor and patch versions)
- Automatic labels: `dependencies`, `github-actions`

### Auto-merge
With the `dependabot-auto-merge.yml` workflow, patch version updates are automatically merged.

## GitHub Actions

### Reusable Workflows

All workflows are designed to be reusable (`workflow_call` trigger) and can be referenced directly in other repositories.

### Permission Configuration

Each workflow is configured with minimal required permissions:
- `contents: read` - Read repository contents
- `packages: write` - Publish packages (publish workflows only)
- `security-events: write` - Upload security scan results
- `checks: write` - Publish test results
- `pull-requests: write` - Comment on PRs

### Secrets Management

When using workflows from this repository, you may need the following secrets:
- `GITHUB_TOKEN` - Automatically provided, used for basic operations
- Other third-party service tokens (if needed)

## Branch Protection

Recommended configuration:
- Protect `main` branch
- Require PR reviews
- Require status checks to pass
- Prohibit force pushes

## Tags and Releases

### Version Tags
Use semantic versioning: `v1.0.0`, `v1.1.0`, `v2.0.0`

### Release Creation
- Create Git tags
- Publish GitHub Release
- Document changes in Release notes

## Issue and PR Templates

You can add the following templates:
- `.github/ISSUE_TEMPLATE/` - Issue templates
- `.github/PULL_REQUEST_TEMPLATE.md` - PR template

## Project Settings Recommendations

1. **Enable Features**
   - Issues
   - Projects
   - Wiki (optional)
   - Discussions (optional)

2. **Actions Settings**
   - Allow all actions and reusable workflows
   - Enable workflow permission management

3. **Code Security**
   - Enable Dependabot alerts
   - Enable Dependabot security updates
   - Enable Code scanning (if using CodeQL)

4. **Access Permissions**
   - Set appropriate team permissions
   - Configure CODEOWNERS file (optional)
