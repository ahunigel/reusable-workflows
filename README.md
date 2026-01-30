# Reusable GitHub Actions Workflows

[![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-2088FF?logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Maintained](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/nigeland/reusable-workflows/graphs/commit-activity)

A collection of reusable GitHub Actions workflows for Java projects CI/CD pipelines.

## 🎯 Features

- ✅ **Parameterized Configuration** - Flexible workflow configuration with customizable JDK versions, build arguments, etc.
- ✅ **Maven & Gradle Support** - Comprehensive support for both major build tools
- ✅ **Multiple JDK Versions** - Support for JDK 8, 11, 17, 21, and custom versions
- ✅ **Code Quality Checks** - Integrated coverage, static analysis, and code style checks
- ✅ **Security Scanning** - OWASP dependency checking and Trivy vulnerability scanning
- ✅ **Test Reports** - Automated test report generation and publishing
- ✅ **Jenkins Plugin Support** - Dedicated workflows for Jenkins plugin development
- ✅ **Complete Documentation** - Comprehensive usage guides, examples, and best practices

## 🚀 Quick Start

> **Note**: Before using these workflows, create a version tag (see [Setup Instructions](#setup-instructions) below).

```yaml
# Create .github/workflows/ci.yml in your project
name: CI

on: [push, pull_request]

jobs:
  build:
    uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
```

Replace `nigeland/reusable-workflows` with your actual repository path.

See the [Quick Start Guide](QUICKSTART.md) for more details.

## 📦 Available Workflows

### Build Workflows

#### Maven Builds
- **maven.yml** - Basic Maven build (JDK 8)
- **maven-hpi-jdk11.yml** - Jenkins plugin build (JDK 11)
- **maven-hpi-jdk17.yml** - Jenkins plugin build (JDK 17)
- **maven-hpi-jdk21.yml** - Jenkins plugin build (JDK 21)

#### Gradle Builds
- **gradle.yml** - Basic Gradle build (JDK 8)
- **gradle-jdk11.yml** - Gradle build (JDK 11)
- **gradle-jdk17.yml** - Gradle build (JDK 17)
- **gradle-jdk21.yml** - Gradle build (JDK 21)

### Publish Workflows

#### Maven Publishing
- **maven-publish.yml** - Publish to GitHub Packages (JDK 8)
- **maven-publish-jdk11.yml** - Publish to GitHub Packages (JDK 11)
- **maven-publish-jdk17.yml** - Publish to GitHub Packages (JDK 17)
- **maven-publish-jdk21.yml** - Publish to GitHub Packages (JDK 21)

#### Gradle Publishing
- **gradle-publish.yml** - Gradle publish (JDK 8)
- **gradle-publish-jdk11.yml** - Gradle publish (JDK 11)
- **gradle-publish-jdk17.yml** - Gradle publish (JDK 17)
- **gradle-publish-jdk21.yml** - Gradle publish (JDK 21)

### Utility Workflows

- **dependabot-auto-merge.yml** - Auto-merge Dependabot patch version updates

## 🚀 Usage

### Basic Usage

Create `.github/workflows/ci.yml` in your project repository:

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    uses: <your-org>/reusable-workflows/.github/workflows/maven.yml@main
```

### Maven Project Example

```yaml
name: Maven Build

on: [push, pull_request]

jobs:
  build-jdk11:
    uses: <your-org>/reusable-workflows/.github/workflows/maven-hpi-jdk11.yml@main
  
  build-jdk17:
    uses: <your-org>/reusable-workflows/.github/workflows/maven-hpi-jdk17.yml@main
```

### Gradle Project Example

```yaml
name: Gradle Build

on: [push, pull_request]

jobs:
  build:
    uses: <your-org>/reusable-workflows/.github/workflows/gradle-jdk17.yml@main
```

### Publishing to GitHub Packages

```yaml
name: Publish

on:
  release:
    types: [created]

jobs:
  publish:
    uses: <your-org>/reusable-workflows/.github/workflows/maven-publish-jdk17.yml@main
```

### Dependabot Auto-merge

Add `.github/workflows/dependabot-auto-merge.yml` in your repository:

```yaml
name: Dependabot auto-merge

on: pull_request

jobs:
  auto-merge:
    uses: <your-org>/reusable-workflows/.github/workflows/dependabot-auto-merge.yml@main
```

## 📝 Configuration

### Maven Workflows
- Uses `mvnw` (Maven Wrapper) by default
- Automatically caches Maven dependencies
- Executes `mvn -B package` build
- Submits dependency graph to GitHub (for Dependabot)

### Gradle Workflows
- Uses `gradlew` (Gradle Wrapper) by default
- Uses `gradle/gradle-build-action` for improved performance
- Executes `gradle build`

### Jenkins Plugin Workflows
- Configures Jenkins repository mirrors
- Supports Jenkins plugin-specific build requirements

## 🔧 Dependency Management

The repository is configured with Dependabot to automatically update GitHub Actions versions weekly:
- Check `.github/dependabot.yml` for configuration
- Patch version updates are automatically merged (requires auto-merge workflow)

## 📋 Best Practices

1. **Version Pinning**: Use specific commit SHA or tag versions instead of `@main`
   ```yaml
   uses: <your-org>/reusable-workflows/.github/workflows/maven.yml@v1.0.0
   ```

2. **Multi-JDK Testing**: Run builds on different JDK versions to ensure compatibility
   ```yaml
   jobs:
     build-jdk11:
       uses: ./.github/workflows/maven-hpi-jdk11.yml@main
     build-jdk17:
       uses: ./.github/workflows/maven-hpi-jdk17.yml@main
   ```

3. **Minimal Permissions**: Set permissions as needed when calling workflows

## 🤝 Contributing

Issues and Pull Requests are welcome to improve these workflows!

## � Setup Instructions

### For Repository Maintainers

After setting up this repository, create version tags:

```bash
# Create and push version tags
git tag v1.0.0
git tag v1
git push origin v1.0.0
git push origin v1
```

**Tag Strategy:**
- `v1.0.0` - Exact version (never moves)
- `v1` - Major version tag (update this when releasing new patches: `git tag -f v1 && git push -f origin v1`)

### For Users

Reference workflows using version tags:

```yaml
# Recommended: Use major version tag (gets latest patches automatically)
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1

# Or: Use exact version (never changes)
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1.0.0
```

## �📄 License

Please add an appropriate license file based on your needs.
