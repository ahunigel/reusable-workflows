# Contributing Guide

Thank you for your interest in contributing to reusable-workflows! We welcome all forms of contributions.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Workflow Development Guidelines](#workflow-development-guidelines)
- [Commit Conventions](#commit-conventions)
- [Testing Guide](#testing-guide)

## Code of Conduct

All participants in this project should follow these principles:
- Respect all contributors
- Accept constructive criticism
- Focus on what's best for the community
- Show empathy towards other community members

## How to Contribute

### Reporting Issues

If you find a bug or have a feature suggestion:

1. Check if a related issue already exists
2. If not, create a new issue
3. Provide detailed information:
   - Issue description
   - Steps to reproduce
   - Expected behavior
   - Actual behavior
   - Environment information (OS, GitHub Actions version, etc.)
   - Relevant logs

### Submitting Code

1. **Fork the Repository**
   ```bash
   # Fork and clone to local
   git clone https://github.com/your-username/reusable-workflows.git
   cd reusable-workflows
   ```

2. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

3. **Make Changes**
   - Follow project coding standards
   - Add necessary comments
   - Update related documentation

4. **Test Your Changes**
   - Test the workflow in an actual project
   - Ensure existing functionality isn't broken

5. **Commit Changes**
   ```bash
   git add .
   git commit -m "feat: add new feature"
   git push origin feature/your-feature-name
   ```

6. **Create Pull Request**
   - Provide a clear PR description
   - Link related issues
   - Wait for code review

## Development Workflow

### Branch Strategy

- `main` - Stable version for production
- `develop` - Development branch
- `feature/*` - New feature branches
- `fix/*` - Bug fix branches
- `docs/*` - Documentation update branches

### Version Releases

We use Semantic Versioning: `MAJOR.MINOR.PATCH`

- **MAJOR**: Incompatible API changes
- **MINOR**: Backward-compatible functionality additions
- **PATCH**: Backward-compatible bug fixes

## Workflow Development Guidelines

### File Naming

- Use lowercase letters and hyphens
- Descriptive naming: `maven-build.yml`, `code-quality.yml`
- Specific JDK versions: `maven-jdk17.yml`

### Workflow Structure

```yaml
# Clear, descriptive name
name: Descriptive Workflow Name

# workflow_call trigger (required for reusable workflows)
on:
  workflow_call:
    inputs:
      # Input parameter definitions
      param-name:
        description: 'Parameter description'
        required: false
        type: string
        default: 'default-value'

# Define permissions when necessary
permissions:
  contents: read

jobs:
  job-name:
    runs-on: ubuntu-latest
    
    steps:
    # Clear step names
    - name: Step description
      # Specify version when using actions
      uses: actions/checkout@v6
      
    # Add necessary comments
    - name: Build step
      run: |
        # Explain complex logic with comments
        command here
```

### Best Practices

1. **Use Latest Stable Action Versions**
   ```yaml
   - uses: actions/checkout@v6  # ✅ Good
   - uses: actions/checkout@v4  # ❌ Outdated
   ```

2. **Provide Reasonable Defaults**
   ```yaml
   inputs:
     java-version:
       default: '17'  # Current LTS version
   ```

3. **Parameter Validation and Error Handling**
   ```yaml
   - name: Validate inputs
     run: |
       if [ -z "${{ inputs.required-param }}" ]; then
         echo "Error: required-param is missing"
         exit 1
       fi
   ```

4. **Use Caching to Improve Performance**
   ```yaml
   - uses: actions/setup-java@v5
     with:
       cache: maven  # or gradle
   ```

5. **Conditional Execution**
   ```yaml
   - name: Optional step
     if: inputs.enable-feature
     run: command
   ```

6. **Clear Output Messages**
   ```yaml
   - name: Build
     run: |
       echo "Building with Java ${{ inputs.java-version }}"
       mvn package
   ```

### Parameter Design Principles

1. **Required vs Optional**: Prefer optional parameters with defaults
2. **Type Safety**: Use correct types (string, number, boolean)
3. **Clear Descriptions**: Every parameter needs a description
4. **Backward Compatibility**: Maintain compatibility when adding new parameters

### Documentation Requirements

Each new workflow must:

1. **Add to README.md**
   ```markdown
   - **workflow-name.yml** - Brief description
   ```

2. **Add Examples to EXAMPLES.md**
   ```yaml
   # Usage example
   jobs:
     example:
       uses: .../workflow-name.yml@v1
       with:
         param: value
   ```

3. **Update STRUCTURE.md**

4. **Record in CHANGELOG.md**

## Commit Conventions

Use [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation updates
- `style`: Code formatting (doesn't affect code execution)
- `refactor`: Refactoring
- `test`: Test-related
- `chore`: Build process or auxiliary tool changes

### Examples

```bash
feat(maven): add code coverage support

Add JaCoCo integration for Maven builds with configurable
coverage thresholds.

Closes #123
```

```bash
fix(gradle): resolve cache key generation issue

Fixed issue where Gradle cache was not properly restored
in multi-module projects.

Fixes #456
```

```bash
docs(readme): update usage examples

- Add multi-JDK matrix example
- Fix typo in parameters section
- Add troubleshooting guide
```

## Testing Guide

### Local Testing

1. **Create Test Repository**
   - Fork or create a test project
   - Configure workflow to call your branch

2. **Test Workflow**
   ```yaml
   jobs:
     test:
       uses: your-username/reusable-workflows/.github/workflows/your-workflow.yml@your-branch
       with:
         test-param: value
   ```

3. **Verify Functionality**
   - Check all steps execute correctly
   - Verify outputs and artifacts
   - Test error handling

### Testing Checklist

Before submitting a PR, ensure:

- [ ] Workflow runs successfully in test project
- [ ] All parameters have been tested
- [ ] Error cases are handled correctly
- [ ] Documentation has been updated
- [ ] Example code works properly
- [ ] Backward compatibility with existing workflows is maintained

## Pull Request Checklist

Before submitting a PR, check:

- [ ] Code follows project standards
- [ ] All tests pass
- [ ] Documentation updated (README, EXAMPLES, STRUCTURE, CHANGELOG)
- [ ] Commit messages follow conventions
- [ ] PR description is clear and complete
- [ ] Related issues are linked

## Code Review

All PRs require code review:

1. **Automated Checks**: GitHub Actions automatically runs checks
2. **Manual Review**: Maintainers review code and documentation
3. **Change Requests**: Make modifications based on feedback
4. **Merge**: Merge to main branch after approval

### Review Focus Areas

- Code quality and readability
- Following best practices
- Documentation completeness
- Backward compatibility
- Security considerations

## Getting Help

If you have any questions:

- 💬 Ask in issues
- 📧 Contact maintainers
- 📖 Review [GitHub Actions Documentation](https://docs.github.com/en/actions)
- 🔍 Search existing issues and PRs

## Thank You Contributors

Thank you to all developers who have contributed to this project!

---

**Thank you again for your contribution!** 🎉
