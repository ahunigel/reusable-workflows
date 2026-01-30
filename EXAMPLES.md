# Usage Examples

This document provides detailed examples of using these reusable workflows.

## 📖 Table of Contents

- [Basic Builds](#basic-builds)
- [Parameterized Builds](#parameterized-builds)
- [Testing and Reporting](#testing-and-reporting)
- [Code Quality Checks](#code-quality-checks)
- [Security Scanning](#security-scanning)
- [Multi-JDK Matrix Testing](#multi-jdk-matrix-testing)
- [Complete CI/CD Pipeline](#complete-cicd-pipeline)

## Basic Builds

### Maven Build with Fixed JDK Version

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    uses: your-org/reusable-workflows/.github/workflows/maven.yml@v1
```

### Gradle Build with Fixed JDK Version

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    uses: your-org/reusable-workflows/.github/workflows/gradle-jdk17.yml@v1
```

## Parameterized Builds

### Maven Parameterized Build Example

```yaml
name: Maven Build

on: [push, pull_request]

jobs:
  build:
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      java-distribution: 'temurin'
      maven-args: '-B clean package -DskipTests=false'
      working-directory: '.'
      upload-artifact: true
      artifact-name: 'my-app-jar'
      artifact-path: 'target/*.jar'
      run-tests: true
      submit-dependency-graph: true
```

### Gradle Parameterized Build Example

```yaml
name: Gradle Build

on: [push, pull_request]

jobs:
  build:
    uses: your-org/reusable-workflows/.github/workflows/gradle-build.yml@v1
    with:
      java-version: '21'
      gradle-tasks: 'clean build'
      upload-artifact: true
      artifact-name: 'gradle-build'
      run-tests: true
      generate-dependency-graph: true
```

## Testing and Reporting

### Run Tests and Generate Reports

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    uses: your-org/reusable-workflows/.github/workflows/maven-test-report.yml@v1
    with:
      java-version: '17'
      working-directory: '.'
      maven-args: '-Dtest=*Test'
```

## Code Quality Checks

### Complete Code Quality Check (Maven Project)

```yaml
name: Quality Check

on:
  pull_request:
    branches: [ main ]

jobs:
  quality:
    uses: your-org/reusable-workflows/.github/workflows/code-quality.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      min-coverage: 80
      enable-spotbugs: true
      enable-checkstyle: true
```

### Code Quality Check (Gradle Project)

```yaml
name: Quality Check

on:
  pull_request:
    branches: [ main ]

jobs:
  quality:
    uses: your-org/reusable-workflows/.github/workflows/code-quality.yml@v1
    with:
      java-version: '21'
      build-tool: 'gradle'
      min-coverage: 75
      enable-spotbugs: true
      enable-checkstyle: false
```

## Security Scanning

### Scheduled Security Scan

```yaml
name: Security Scan

on:
  schedule:
    # Run every Monday at 2 AM
    - cron: '0 2 * * 1'
  workflow_dispatch:

jobs:
  security:
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      fail-on-severity: 'CRITICAL'
```

### Security Scan on Pull Requests

```yaml
name: PR Security Check

on:
  pull_request:
    branches: [ main, develop ]

jobs:
  security-check:
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      build-tool: 'gradle'
      fail-on-severity: 'HIGH'
```

## Multi-JDK Matrix Testing

### Test on Multiple JDK Versions

```yaml
name: Compatibility Test

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  matrix-test:
    uses: your-org/reusable-workflows/.github/workflows/multi-jdk-matrix.yml@v1
    with:
      build-tool: 'maven'
      jdk-versions: '["11", "17", "21"]'
      java-distribution: 'temurin'
```

### Custom JDK Version Combinations

```yaml
name: Extended Compatibility Test

on:
  schedule:
    - cron: '0 0 * * 0'  # Run every Sunday

jobs:
  matrix-test:
    uses: your-org/reusable-workflows/.github/workflows/multi-jdk-matrix.yml@v1
    with:
      build-tool: 'gradle'
      jdk-versions: '["8", "11", "17", "21", "22"]'
      java-distribution: 'temurin'
```

## Complete CI/CD Pipeline

### Comprehensive CI Pipeline Example

```yaml
name: Complete CI Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  # Stage 1: Build
  build:
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      maven-args: '-B package'
      upload-artifact: true
      artifact-name: 'build-artifacts'
      
  # Stage 2: Test
  test:
    needs: build
    uses: your-org/reusable-workflows/.github/workflows/maven-test-report.yml@v1
    with:
      java-version: '17'
      
  # Stage 3: Code Quality
  quality:
    needs: build
    uses: your-org/reusable-workflows/.github/workflows/code-quality.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      min-coverage: 80
      
  # Stage 4: Security Scan
  security:
    needs: build
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      fail-on-severity: 'HIGH'
      
  # Stage 5: Multi-Version Compatibility Test
  compatibility:
    needs: [test, quality, security]
    uses: your-org/reusable-workflows/.github/workflows/multi-jdk-matrix.yml@v1
    with:
      build-tool: 'maven'
      jdk-versions: '["11", "17", "21"]'
```

### Release Pipeline Example

```yaml
name: Release Pipeline

on:
  release:
    types: [published]

jobs:
  # Build and test
  build-and-test:
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      maven-args: '-B clean verify'
      upload-artifact: true
      
  # Security check
  security-check:
    needs: build-and-test
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
    with:
      java-version: '17'
      build-tool: 'maven'
      fail-on-severity: 'CRITICAL'
      
  # Publish to GitHub Packages
  publish:
    needs: [build-and-test, security-check]
    uses: your-org/reusable-workflows/.github/workflows/maven-publish-jdk17.yml@v1
```

## Jenkins Plugin Project Example

### Jenkins Plugin CI

```yaml
name: Jenkins Plugin CI

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  build-jdk11:
    uses: your-org/reusable-workflows/.github/workflows/maven-hpi-jdk11.yml@v1
    
  build-jdk17:
    uses: your-org/reusable-workflows/.github/workflows/maven-hpi-jdk17.yml@v1
    
  build-jdk21:
    uses: your-org/reusable-workflows/.github/workflows/maven-hpi-jdk21.yml@v1
```

## Dependabot Auto-Merge

### Configure Auto-Merge for Dependabot PRs

Create `.github/workflows/dependabot-auto-merge.yml` in your repository:

```yaml
name: Dependabot auto-merge

on: pull_request

jobs:
  auto-merge:
    uses: your-org/reusable-workflows/.github/workflows/dependabot-auto-merge.yml@v1
```

## Best Practices

### 1. Use Specific Versions Instead of main Branch

❌ Not recommended:
```yaml
uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@main
```

✅ Recommended:
```yaml
uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1.0.0
```

### 2. Use Different Configurations for Different Environments

```yaml
jobs:
  build-dev:
    if: github.ref != 'refs/heads/main'
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      maven-args: '-B package -DskipTests'
      
  build-prod:
    if: github.ref == 'refs/heads/main'
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      maven-args: '-B clean verify'
      run-tests: true
```

### 3. Parallel Workflow Execution

```yaml
jobs:
  # Execute multiple independent tasks in parallel
  build:
    uses: your-org/reusable-workflows/.github/workflows/maven-build.yml@v1
    
  security:
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
    
  quality:
    uses: your-org/reusable-workflows/.github/workflows/code-quality.yml@v1
```

### 4. Conditional Workflow Execution

```yaml
jobs:
  security-scan:
    # Only run security scan on PRs and main branch
    if: github.event_name == 'pull_request' || github.ref == 'refs/heads/main'
    uses: your-org/reusable-workflows/.github/workflows/security-scan.yml@v1
```

## Troubleshooting

### Common Issues

1. **Permission Errors**: Ensure the workflow has necessary permissions
2. **mvnw/gradlew Not Found**: Confirm wrapper files exist in project root
3. **Dependency Download Failures**: Check network configuration or use mirrors
4. **Test Failures**: Review test report artifacts for detailed information

### Getting Help

If you encounter issues:
1. Check GitHub Actions logs
2. Review workflow file comments
3. Submit an issue in the repository
