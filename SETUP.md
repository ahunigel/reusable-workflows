# Setup Guide

## Quick Setup for Repository Maintainers

Follow these steps to set up the reusable-workflows repository:

### 1. Create Version Tags

After committing your workflows, create version tags:

```bash
# Navigate to your repository
cd d:\nigeland\reusable-workflows

# Create exact version tag
git tag -a v1.0.0 -m "Release v1.0.0"

# Create major version tag (will be updated with patches)
git tag -a v1 -m "Release v1"

# Push tags to remote
git push origin v1.0.0
git push origin v1
```

### 2. Tag Strategy Explained

- **`v1.0.0`** - Exact version tag (immutable)
  - Users referencing this will always get this exact version
  - Never move this tag
  
- **`v1`** - Floating major version tag (mutable)
  - Points to the latest v1.x.x release
  - Update this when releasing patches:
    ```bash
    git tag -f v1
    git push -f origin v1
    ```

### 3. Future Releases

When releasing updates:

**Patch Release (v1.0.1):**
```bash
git tag v1.0.1
git tag -f v1          # Move v1 to point to v1.0.1
git push origin v1.0.1
git push -f origin v1  # Force push to move the tag
```

**Minor Release (v1.1.0):**
```bash
git tag v1.1.0
git tag -f v1          # Move v1 to point to v1.1.0
git push origin v1.1.0
git push -f origin v1
```

**Major Release (v2.0.0):**
```bash
git tag v2.0.0
git tag v2             # Create new v2 tag
git push origin v2.0.0
git push origin v2
# Keep v1 pointing to latest v1.x.x for backward compatibility
```

## For Users of This Repository

### How to Reference Workflows

**Option 1: Major Version (Recommended)**
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1
```
✅ Automatically gets bug fixes and patches
✅ Stays within the same major version
✅ Good balance of stability and updates

**Option 2: Exact Version**
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1.0.0
```
✅ Maximum stability (never changes)
✅ Predictable behavior
❌ Requires manual updates for bug fixes

**Option 3: Main Branch (Not Recommended)**
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@main
```
❌ Can break without warning
❌ Not suitable for production
✅ Only for testing/development

## Version Control Best Practices

1. **Always use version tags in production**
2. **Test new versions in development branches first**
3. **Document breaking changes in CHANGELOG.md**
4. **Follow semantic versioning (semver.org)**

## Quick Commands Reference

```bash
# List all tags
git tag -l

# Delete a local tag
git tag -d v1.0.0

# Delete a remote tag
git push origin --delete v1.0.0

# View tag details
git show v1.0.0

# Check what commit a tag points to
git rev-list -n 1 v1
```

## Troubleshooting

### "Workflow not found" Error

Make sure:
1. Tags are pushed to remote: `git push origin v1`
2. Workflows exist in the tagged version
3. Repository path is correct in `uses:` statement

### Tags Not Showing in Other Repositories

After creating tags, users need to reference them correctly:
```yaml
# Wrong
uses: reusable-workflows/.github/workflows/maven-build.yml@v1

# Correct
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1
```

## Summary

**Your TODO List:**
- [ ] Commit all workflow files
- [ ] Create `v1.0.0` tag
- [ ] Create `v1` tag  
- [ ] Push both tags to remote
- [ ] Update documentation examples with your actual org/repo name
- [ ] Test workflows from another repository

That's it! Your reusable workflows are now ready to use.
