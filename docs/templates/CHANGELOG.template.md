---
description: "[Internal] Changelog template - use /doc-gen instead"
disable-model-invocation: true
---

# Changelog Template

Template for generating and maintaining CHANGELOG.md files. Variables use mustache-style syntax: `{{VARIABLE_NAME}}`.

---

## Template

```markdown
# Changelog

All notable changes to {{PROJECT_NAME}} will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
{{#UNRELEASED_ADDED}}
- {{CHANGE_DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/UNRELEASED_ADDED}}

### Changed
{{#UNRELEASED_CHANGED}}
- {{CHANGE_DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/UNRELEASED_CHANGED}}

### Deprecated
{{#UNRELEASED_DEPRECATED}}
- {{CHANGE_DESCRIPTION}}
{{/UNRELEASED_DEPRECATED}}

### Removed
{{#UNRELEASED_REMOVED}}
- {{CHANGE_DESCRIPTION}}
{{/UNRELEASED_REMOVED}}

### Fixed
{{#UNRELEASED_FIXED}}
- {{CHANGE_DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/UNRELEASED_FIXED}}

### Security
{{#UNRELEASED_SECURITY}}
- {{CHANGE_DESCRIPTION}}
{{/UNRELEASED_SECURITY}}

{{#VERSIONS}}
## [{{VERSION}}] - {{DATE}}

{{#HAS_SUMMARY}}
{{VERSION_SUMMARY}}
{{/HAS_SUMMARY}}

{{#ADDED}}
### Added
{{#ITEMS}}
- {{DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/ITEMS}}
{{/ADDED}}

{{#CHANGED}}
### Changed
{{#ITEMS}}
- {{DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/ITEMS}}
{{/CHANGED}}

{{#DEPRECATED}}
### Deprecated
{{#ITEMS}}
- {{DESCRIPTION}}
{{/ITEMS}}
{{/DEPRECATED}}

{{#REMOVED}}
### Removed
{{#ITEMS}}
- {{DESCRIPTION}}
{{/ITEMS}}
{{/REMOVED}}

{{#FIXED}}
### Fixed
{{#ITEMS}}
- {{DESCRIPTION}} {{#ISSUE_REF}}([#{{ISSUE_NUMBER}}]({{ISSUES_URL}}/{{ISSUE_NUMBER}})){{/ISSUE_REF}}
{{/ITEMS}}
{{/FIXED}}

{{#SECURITY}}
### Security
{{#ITEMS}}
- {{DESCRIPTION}}
{{/ITEMS}}
{{/SECURITY}}

{{/VERSIONS}}

{{#HAS_LINKS}}
[Unreleased]: {{REPOSITORY_URL}}/compare/v{{LATEST_VERSION}}...HEAD
{{#VERSION_LINKS}}
[{{VERSION}}]: {{REPOSITORY_URL}}/compare/v{{PREVIOUS_VERSION}}...v{{VERSION}}
{{/VERSION_LINKS}}
[{{FIRST_VERSION}}]: {{REPOSITORY_URL}}/releases/tag/v{{FIRST_VERSION}}
{{/HAS_LINKS}}
```

---

## Variable Reference

### Project Variables

| Variable | Type | Description |
|----------|------|-------------|
| `PROJECT_NAME` | string | Name of the project |
| `REPOSITORY_URL` | string | Repository URL |
| `ISSUES_URL` | string | Issues page URL |

### Version Variables

| Variable | Type | Description |
|----------|------|-------------|
| `VERSIONS` | array | List of version entries |
| `VERSION` | string | Version number (1.0.0) |
| `DATE` | string | Release date (YYYY-MM-DD) |
| `VERSION_SUMMARY` | string | Optional version summary |
| `LATEST_VERSION` | string | Most recent version |
| `FIRST_VERSION` | string | First released version |

### Change Categories

| Variable | Type | Description |
|----------|------|-------------|
| `ADDED` | array | New features |
| `CHANGED` | array | Changes in existing functionality |
| `DEPRECATED` | array | Soon-to-be removed features |
| `REMOVED` | array | Removed features |
| `FIXED` | array | Bug fixes |
| `SECURITY` | array | Security fixes |

### Change Item Variables

| Variable | Type | Description |
|----------|------|-------------|
| `DESCRIPTION` | string | Description of the change |
| `ISSUE_NUMBER` | number | Related issue number |
| `ISSUE_REF` | boolean | Whether to include issue reference |

---

## Change Category Guidelines

### Added
Use for new features that add functionality.

**Examples:**
- Add user authentication via OAuth2
- Add dark mode support
- Add API endpoint for bulk operations
- Add support for PostgreSQL 16
- Add localization for Spanish

### Changed
Use for changes in existing functionality.

**Examples:**
- Update minimum Node.js version to 18
- Improve performance of search algorithm by 40%
- Refactor authentication module for better maintainability
- Change default timeout from 30s to 60s
- Update dependencies to latest versions

### Deprecated
Use for features that will be removed in future versions.

**Examples:**
- Deprecate `oldFunction()` in favor of `newFunction()`
- Deprecate support for Node.js 16 (will be removed in v3.0)
- Deprecate `config.legacyMode` option
- Deprecate `/api/v1/` endpoints (use `/api/v2/`)

### Removed
Use for features that have been removed.

**Examples:**
- Remove deprecated `legacyMode` option
- Remove support for Node.js 14
- Remove `/api/v1/` endpoints
- Remove unused dependencies

### Fixed
Use for bug fixes.

**Examples:**
- Fix memory leak in connection pool
- Fix incorrect calculation in billing module
- Fix race condition in async operations
- Fix typo in error message
- Fix incorrect timezone handling

### Security
Use for security-related changes.

**Examples:**
- Fix XSS vulnerability in user input handling
- Update `lodash` to address CVE-2021-23337
- Add rate limiting to authentication endpoints
- Fix SQL injection vulnerability in search
- Implement CSRF protection

---

## Version Numbering (SemVer)

### Format: MAJOR.MINOR.PATCH

| Component | When to Increment |
|-----------|-------------------|
| MAJOR | Breaking changes, incompatible API changes |
| MINOR | New features, backward-compatible |
| PATCH | Bug fixes, backward-compatible |

### Pre-release Versions

| Format | Use Case |
|--------|----------|
| `1.0.0-alpha.1` | Early testing, unstable |
| `1.0.0-beta.1` | Feature complete, testing |
| `1.0.0-rc.1` | Release candidate |

---

## Example Changelog

```markdown
# Changelog

All notable changes to My Project will be documented in this file.

## [Unreleased]

### Added
- Add support for custom themes
- Add API endpoint for user preferences

### Fixed
- Fix pagination issue on search results

## [2.1.0] - 2025-01-15

This release focuses on performance improvements and new features.

### Added
- Add dark mode support ([#234](https://github.com/user/repo/issues/234))
- Add bulk export functionality ([#256](https://github.com/user/repo/issues/256))
- Add keyboard shortcuts for common actions

### Changed
- Improve search performance by 3x ([#245](https://github.com/user/repo/issues/245))
- Update to React 18

### Fixed
- Fix memory leak in WebSocket connections ([#251](https://github.com/user/repo/issues/251))
- Fix incorrect date formatting in reports

## [2.0.0] - 2024-12-01

### Breaking Changes
- Minimum Node.js version is now 18
- Remove deprecated v1 API endpoints

### Added
- Add new v2 API with improved performance
- Add support for PostgreSQL 16

### Changed
- Migrate from Express to Fastify
- Update all dependencies to latest versions

### Removed
- Remove Node.js 16 support
- Remove deprecated `config.legacyMode`

### Security
- Update `jsonwebtoken` to fix CVE-2023-XXXXX

## [1.5.0] - 2024-10-15

### Added
- Add user profile customization
- Add export to PDF feature

### Fixed
- Fix login redirect issue ([#189](https://github.com/user/repo/issues/189))

## [1.0.0] - 2024-08-01

### Added
- Initial release
- User authentication and authorization
- Basic CRUD operations
- REST API
- Documentation

[Unreleased]: https://github.com/user/repo/compare/v2.1.0...HEAD
[2.1.0]: https://github.com/user/repo/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/user/repo/compare/v1.5.0...v2.0.0
[1.5.0]: https://github.com/user/repo/compare/v1.0.0...v1.5.0
[1.0.0]: https://github.com/user/repo/releases/tag/v1.0.0
```

---

## Generating Changelog from Git

### Using Conventional Commits

With conventional commits, you can auto-generate changelog entries:

```bash
# Example: extract features since last tag
git log --oneline --grep="^feat" $(git describe --tags --abbrev=0)..HEAD

# Example: extract fixes since last tag
git log --oneline --grep="^fix" $(git describe --tags --abbrev=0)..HEAD
```

### Commit to Changelog Mapping

| Commit Type | Changelog Section |
|-------------|-------------------|
| `feat` | Added |
| `fix` | Fixed |
| `perf` | Changed |
| `refactor` | Changed |
| `docs` | (usually not included) |
| `security` | Security |
| `BREAKING CHANGE` | Changed (major) |

---

## Best Practices

1. **Update on every release** - Keep changelog current
2. **Be user-focused** - Write for people using the software
3. **Be concise** - Short, clear descriptions
4. **Link issues** - Reference related issues/PRs
5. **Group logically** - Use the standard categories
6. **Keep Unreleased** - Track work in progress
7. **Date format** - Use ISO format (YYYY-MM-DD)
8. **Latest first** - Most recent version at top
9. **Include links** - Link to version comparisons
10. **Highlight breaking** - Make breaking changes obvious
