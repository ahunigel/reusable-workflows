# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.0.0] - 2026-01-30

### Added

#### New Parameterized Workflows
- **maven-build.yml** - Parameterized Maven build with customizable JDK versions, Maven arguments, and artifact upload
- **gradle-build.yml** - Parameterized Gradle build with flexible task configuration and dependency graph generation
- **maven-test-report.yml** - Maven test execution with automatic test report generation and publishing to PRs
- **code-quality.yml** - Comprehensive code quality checks including JaCoCo coverage, SpotBugs, and Checkstyle
- **security-scan.yml** - Security vulnerability scanning with OWASP dependency check and Trivy
- **multi-jdk-matrix.yml** - Matrix testing across multiple JDK versions with parallel execution

#### Documentation
- **README.md** - Comprehensive project documentation with usage guides
- **QUICKSTART.md** - 5-minute quick start guide for rapid onboarding
- **EXAMPLES.md** - Detailed usage examples covering various scenarios
- **STRUCTURE.md** - Project structure documentation and file organization
- **CHANGELOG.md** - Version history and release notes (this file)
- **CONTRIBUTING.md** - Contribution guidelines and development workflow
- **IMPROVEMENTS.md** - Detailed improvements analysis and recommendations
- **LICENSE** - MIT License
- **.gitignore** - Git ignore configuration
- **.github/GITHUB_CONFIG.md** - GitHub-specific configuration guide

### Changed

#### Enhanced Dependabot Configuration
- Added scheduled update time (Monday 9:00 AM)
- Configured PR limit (10 concurrent PRs)
- Added automatic labels: `dependencies`, `github-actions`
- Implemented grouped updates for minor and patch versions
- Standardized commit message format with `chore(deps)` prefix

#### Updated Actions Versions
- actions/checkout → v6
- actions/setup-java → v5
- actions/upload-artifact → v4
- gradle/gradle-build-action → v3
- All other actions updated to latest stable versions

### Improvements

#### Code Reduction
- Reduced code duplication by ~70% through parameterization
- Single workflow can replace multiple version-specific files
- Unified interface for easier maintenance

#### Enhanced Flexibility
- Customizable JDK versions (any version, not just 8, 11, 17, 21)
- Configurable build commands and arguments
- Optional features with boolean flags
- Custom working directories support
- Artifact upload with configurable names and paths

#### Quality Assurance
- Automated code coverage reporting with configurable thresholds
- Static analysis with SpotBugs
- Code style checks with Checkstyle
- Coverage reports displayed in PRs via JaCoCo
- Support for both Maven and Gradle

#### Security
- OWASP dependency vulnerability checks
- Trivy security scanning
- SARIF format reports integrated with GitHub Security
- Configurable severity failure levels

#### Testing
- Automated test report generation
- Test results published to PRs
- Support for Surefire and Failsafe reports
- Multi-JDK version compatibility testing
- Parallel execution across JDK versions

### Fixed
- Fixed mvnw/gradlew permission issues in various workflows
- Improved caching strategy for faster builds
- Enhanced error handling and reporting

### Security
- Updated all GitHub Actions to latest stable versions
- Implemented minimal permissions principle
- Added security scanning workflows

## [Unreleased]

### Planned Features
- Docker build and publish workflow
- Kubernetes deployment workflow
- SonarQube integration
- Performance testing workflow
- Notification integrations (Slack, Teams, DingTalk)
- Automatic release notes generation
- Monorepo support

## Version History

### v1.0.0 (2026-01-30)
Initial release with comprehensive workflow collection and complete documentation.

## Migration Notes

### From Pre-1.0 to 1.0.0

**No Breaking Changes** - All existing workflows remain fully compatible.

#### For New Projects
Use the new parameterized workflows for maximum flexibility:
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-build.yml@v1.0.0
```

#### For Existing Projects
Continue using existing workflows without any modifications:
```yaml
uses: nigeland/reusable-workflows/.github/workflows/maven-jdk17.yml@v1.0.0
```

#### Recommended Upgrade Path
1. Test new workflows in a development branch
2. Gradually migrate to parameterized workflows
3. Add quality and security checks
4. Implement complete CI/CD pipeline

## Support

- **Issues**: [GitHub Issues](https://github.com/nigeland/reusable-workflows/issues)
- **Discussions**: [GitHub Discussions](https://github.com/nigeland/reusable-workflows/discussions)
- **Documentation**: [README.md](README.md), [EXAMPLES.md](EXAMPLES.md)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
