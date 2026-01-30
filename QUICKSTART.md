# Quick Start Guide

This guide helps you get started with reusable-workflows quickly.

> **Important**: Replace `ahunigel/reusable-workflows` with your actual repository path and ensure version tags are created (see README Setup Instructions).

## 🚀 5-Minute Quick Start

### Step 1: Choose the Right Workflow

Select based on your project type:

| Project Type | Recommended Workflow | Example Link |
|-------------|---------------------|--------------|
| Simple Maven Project | maven-build.yml | [Example](#maven-project-quick-start) |
| Simple Gradle Project | gradle-build.yml | [Example](#gradle-project-quick-start) |
| Jenkins Plugin | maven-hpi-jdk*.yml | [Example](#jenkins-plugin-quick-start) |
| Complete CI Needed | Combine multiple workflows | [Example](#complete-ci-quick-start) |

### Step 2: Create Workflow File

Create `.github/workflows/ci.yml` in your project

### Step 3: Configure and Run

Commit your code and GitHub Actions will run automatically.

---

## Maven Project Quick Start

### Minimal Configuration

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
```

### With Artifact Upload

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      upload-artifact: true
      artifact-name: 'my-app'
      artifact-path: 'target/*.jar'
```

---

## Gradle Project Quick Start

### Minimal Configuration

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    uses: ahunigel/reusable-workflows/.github/workflows/gradle-build.yml@v1
    with:
      java-version: '17'
```

### Custom Gradle Tasks

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    uses: ahunigel/reusable-workflows/.github/workflows/gradle-build.yml@v1
    with:
      java-version: '21'
      gradle-tasks: 'clean build'
      upload-artifact: true
```

---

## Jenkins Plugin Quick Start

### Multi-Version Testing

Create `.github/workflows/ci.yml`:

```yaml
name: Jenkins Plugin CI

on: [push, pull_request]

jobs:
  build-jdk11:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-hpi-jdk11.yml@v1
    
  build-jdk17:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-hpi-jdk17.yml@v1
    
  build-jdk21:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-hpi-jdk21.yml@v1
```

---

## Complete CI Quick Start

### With Tests, Quality Checks, and Security Scanning

Create `.github/workflows/ci.yml`:

```yaml
name: Complete CI

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  # Build
  build:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      upload-artifact: true
  
  # Test Reports
  test:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-test-report.yml@v1
    with:
      java-version: '17'
  
  # Code Quality
  quality:
    uses: ahunigel/reusable-workflows/.github/workflows/code-quality.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      min-coverage: 80
  
  # Security Scanning
  security:
    uses: ahunigel/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
```

---

## Dependabot Auto-Merge

### Enable Auto-Merge

Create `.github/workflows/dependabot-auto-merge.yml`:

```yaml
name: Dependabot auto-merge

on: pull_request

jobs:
  auto-merge:
    uses: ahunigel/reusable-workflows/.github/workflows/dependabot-auto-merge.yml@v1
```

---

## Publish to GitHub Packages

### Auto-Publish on Release

Create `.github/workflows/release.yml`:

```yaml
name: Release

on:
  release:
    types: [published]

jobs:
  publish:
    uses: ahunigel/reusable-workflows/.github/workflows/maven-publish-jdk17.yml@v1
```

---

## Common Scenarios

### Scenario 1: PR Checks

Run quality checks only on PRs:

```yaml
name: PR Check

on:
  pull_request:
    branches: [ main ]

jobs:
  quality:
    uses: ahunigel/reusable-workflows/.github/workflows/code-quality.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
```

### Scenario 2: Scheduled Security Scan

Weekly security scan:

```yaml
name: Weekly Security Scan

on:
  schedule:
    - cron: '0 2 * * 1'  # Every Monday at 2 AM

jobs:
  security:
    uses: ahunigel/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
```

### Scenario 3: Multi-JDK Compatibility Test

Test on multiple JDK versions:

```yaml
name: Compatibility

on: [push, pull_request]

jobs:
  matrix-test:
    uses: ahunigel/reusable-workflows/.github/workflows/multi-jdk-matrix.yml@v1
    with:
      build-tool: 'maven'
      jdk-versions: '["11", "17", "21"]'
```

---

## 🎓 Next Steps

Congratulations! You've mastered the basics. Next:

1. 📖 Read [EXAMPLES.md](EXAMPLES.md) for advanced usage
2. 🏗️ Check [STRUCTURE.md](STRUCTURE.md) to understand project structure
3. 📋 Refer to [README.md](README.md) for complete documentation
4. 🔧 Customize workflow parameters based on your needs

## 💡 Tips

1. **Use Version Tags**: Use `@v1.0.0` instead of `@main` for stability
2. **Start Simple**: Begin with basic workflows, then add features gradually
3. **Check Logs**: View detailed execution logs in the GitHub Actions tab
4. **Test Configuration**: Test workflow configuration in a feature branch first

## ❓ Having Issues?

- Check [FAQ](#) 
- Submit an [Issue](GitHub Issues)
- Review [GitHub Actions Documentation](https://docs.github.com/en/actions)

---

**Tip**: Replace `ahunigel/reusable-workflows` with your actual repository path!
