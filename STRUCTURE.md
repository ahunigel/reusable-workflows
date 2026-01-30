# Project Structure

## 📁 Directory Structure

```
reusable-workflows/
├── .github/
│   ├── dependabot.yml           # Dependabot configuration
│   ├── GITHUB_CONFIG.md          # GitHub configuration guide
│   └── workflows/                # Reusable workflows directory
│       ├── # Basic build workflows (existing, fixed JDK versions)
│       ├── maven.yml
│       ├── maven-hpi-jdk11.yml
│       ├── maven-hpi-jdk17.yml
│       ├── maven-hpi-jdk21.yml
│       ├── gradle.yml
│       ├── gradle-jdk11.yml
│       ├── gradle-jdk17.yml
│       ├── gradle-jdk21.yml
│       │
│       ├── # Publish workflows (existing)
│       ├── maven-publish.yml
│       ├── maven-publish-jdk11.yml
│       ├── maven-publish-jdk17.yml
│       ├── maven-publish-jdk21.yml
│       ├── gradle-publish.yml
│       ├── gradle-publish-jdk11.yml
│       ├── gradle-publish-jdk17.yml
│       ├── gradle-publish-jdk21.yml
│       │
│       ├── # Parameterized workflows (new, recommended)
│       ├── maven-build.yml          # Parameterized Maven build
│       ├── gradle-build.yml         # Parameterized Gradle build
│       │
│       ├── # Testing and quality workflows (new)
│       ├── maven-test-report.yml    # Maven test reports
│       ├── code-quality.yml         # Code quality checks
│       ├── security-scan.yml        # Security scanning
│       ├── multi-jdk-matrix.yml     # Multi-JDK matrix testing
│       │
│       └── # Utility workflows (existing)
│           └── dependabot-auto-merge.yml
│
├── README.md                    # Main documentation
├── QUICKSTART.md                # Quick start guide
├── EXAMPLES.md                  # Usage examples
├── STRUCTURE.md                 # This file
├── CHANGELOG.md                 # Version history
├── CONTRIBUTING.md              # Contribution guidelines
├── IMPROVEMENTS.md              # Improvements summary
├── LICENSE                      # MIT License
└── .gitignore                   # Git ignore file
```

## 📚 File Categories

### 1. Existing Workflows (Backward Compatible)

These workflows remain unchanged for existing project compatibility:

**Maven Builds:**
- `maven.yml` - JDK 8
- `maven-hpi-jdk11.yml` - Jenkins plugin JDK 11
- `maven-hpi-jdk17.yml` - Jenkins plugin JDK 17
- `maven-hpi-jdk21.yml` - Jenkins plugin JDK 21

**Gradle Builds:**
- `gradle.yml` - JDK 8
- `gradle-jdk11.yml` - JDK 11
- `gradle-jdk17.yml` - JDK 17
- `gradle-jdk21.yml` - JDK 21

**Publish Workflows:** (same pattern with publish suffix)

### 2. New Parameterized Workflows (Recommended)

More flexible, configurable workflows:

#### `maven-build.yml`
**Purpose:** Parameterized Maven build
**Input Parameters:**
- `java-version` - JDK version (default 17)
- `java-distribution` - JDK distribution (default temurin)
- `maven-args` - Maven command arguments
- `working-directory` - Working directory
- `upload-artifact` - Whether to upload artifacts
- `artifact-name` - Artifact name
- `artifact-path` - Artifact path
- `run-tests` - Whether to run tests
- `submit-dependency-graph` - Whether to submit dependency graph

#### `gradle-build.yml`
**Purpose:** Parameterized Gradle build
**Input Parameters:**
- `java-version` - JDK version (default 17)
- `java-distribution` - JDK distribution
- `gradle-tasks` - Gradle tasks
- `working-directory` - Working directory
- `upload-artifact` - Whether to upload artifacts
- `artifact-name` - Artifact name
- `artifact-path` - Artifact path
- `run-tests` - Whether to run tests
- `generate-dependency-graph` - Whether to generate dependency graph

#### `maven-test-report.yml`
**Purpose:** Maven testing with report generation
**Features:**
- Publishes test results to PRs
- Uploads test reports as artifacts
- Supports Surefire and Failsafe reports

#### `code-quality.yml`
**Purpose:** Code quality checks
**Features:**
- JaCoCo code coverage checks
- SpotBugs static analysis
- Checkstyle code style checks
- Upload coverage to Codecov
- Display coverage reports in PRs
- Supports both Maven and Gradle

#### `security-scan.yml`
**Purpose:** Security vulnerability scanning
**Features:**
- OWASP dependency checks
- Trivy vulnerability scanning
- Upload SARIF results to GitHub Security
- Configurable failure levels

#### `multi-jdk-matrix.yml`
**Purpose:** Multi-JDK version matrix testing
**Features:**
- Customizable JDK version list
- Parallel testing across versions
- Failures don't affect other versions

### 3. Utility Workflows

#### `dependabot-auto-merge.yml`
**Purpose:** Auto-merge Dependabot patch version updates
**Trigger:** PRs created by Dependabot with patch updates

## 🔄 Migration Guide

### Migrating from Existing to Parameterized Workflows

**Before (maven-hpi-jdk17.yml):**
```yaml
jobs:
  build:
    uses: nigeland/reusable-workflows/.github/workflows/maven-hpi-jdk17.yml@v1
```

**After (maven-build.yml):**
```yaml
jobs:
  build:
    uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1
    with:
      java-version: '17'
      maven-args: '-B package'
```

### Benefits
- ✅ Single workflow replaces multiple version-specific workflows
- ✅ More flexible configuration options
- ✅ Easier to maintain and update
- ✅ Reduced code duplication

## 📊 Usage Recommendations

### Simple Projects
Use existing fixed-version workflows:
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-jdk17.yml@v1
```

### Projects Needing Flexible Configuration
Use new parameterized workflows:
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1
with:
  java-version: '17'
  upload-artifact: true
```

### Enterprise Projects
Combine multiple workflows for complete CI/CD pipeline:
```yaml
jobs:
  build:
    uses: .../maven-build.yml@v1
  test:
    uses: .../maven-test-report.yml@v1
  quality:
    uses: .../code-quality.yml@v1
  security:
    uses: .../security-scan.yml@v1
```

## 🔮 Future Plans

Potential features to add:
- [ ] Docker image build and publish workflow
- [ ] Kubernetes deployment workflow
- [ ] Performance testing workflow
- [ ] Integration testing workflow
- [ ] Documentation generation and publishing workflow
- [ ] Automatic release notes generation
- [ ] Slack/Teams/DingTalk notification integration
- [ ] SonarQube integration

## 🤝 Contribution Guidelines

When adding new workflows:
1. Follow existing naming conventions
2. Provide detailed parameter descriptions
3. Add usage examples to EXAMPLES.md
4. Update README.md
5. Maintain backward compatibility
