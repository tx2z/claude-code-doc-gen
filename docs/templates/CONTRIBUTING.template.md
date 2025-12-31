---
description: "[Internal] Contributing guide template - use /doc-gen instead"
disable-model-invocation: true
---

# Contributing Guide Template

Template for generating CONTRIBUTING.md files. Variables use mustache-style syntax: `{{VARIABLE_NAME}}`.

---

## Template

```markdown
# Contributing to {{PROJECT_NAME}}

Thank you for your interest in contributing to {{PROJECT_NAME}}! This document provides guidelines and information for contributors.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Documentation](#documentation)

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold this code. Please report unacceptable behavior to {{CONTACT_EMAIL}}.

## Getting Started

### Prerequisites

Ensure you have the following installed:
{{#PREREQUISITES}}
- {{PREREQ_NAME}} ({{PREREQ_VERSION}})
{{/PREREQUISITES}}

### Setup

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/{{REPO_NAME}}.git
   cd {{REPO_NAME}}
   ```
3. Add the upstream remote:
   ```bash
   git remote add upstream {{REPOSITORY_URL}}
   ```
4. Install dependencies:
   ```bash
   {{INSTALL_COMMAND}}
   ```
5. Create a branch for your work:
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Development Workflow

### Branch Naming Convention

Use the following prefixes for branches:

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feature/` | New features | `feature/user-authentication` |
| `fix/` | Bug fixes | `fix/login-redirect` |
| `docs/` | Documentation updates | `docs/api-reference` |
| `refactor/` | Code refactoring | `refactor/user-service` |
| `test/` | Adding or updating tests | `test/auth-unit-tests` |
| `chore/` | Maintenance tasks | `chore/update-dependencies` |

### Making Changes

1. Ensure your branch is up to date:
   ```bash
   git fetch upstream
   git rebase upstream/{{DEFAULT_BRANCH}}
   ```

2. Make your changes in small, focused commits

3. Write meaningful commit messages following our [commit conventions](#commit-message-format)

4. Run tests and linting before committing:
   ```bash
   {{TEST_COMMAND}}
   {{LINT_COMMAND}}
   ```

### Commit Message Format

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

**Types:**
| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Code style changes (formatting, semicolons, etc.) |
| `refactor` | Code refactoring (no feature or bug fix) |
| `perf` | Performance improvements |
| `test` | Adding or updating tests |
| `chore` | Maintenance tasks |
| `ci` | CI/CD changes |

**Examples:**
```
feat(auth): add password reset functionality

fix(api): correct user validation error message

docs(readme): update installation instructions
```

## Pull Request Process

### Before Submitting

- [ ] Code follows the project's coding standards
- [ ] All tests pass locally
- [ ] New features include tests
- [ ] Documentation is updated if needed
- [ ] Commit messages follow conventions
- [ ] Branch is rebased on latest {{DEFAULT_BRANCH}}

### Submitting a Pull Request

1. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

2. Open a Pull Request against `{{DEFAULT_BRANCH}}`

3. Fill out the PR template with:
   - Summary of changes
   - Related issue numbers
   - Testing performed
   - Screenshots (if UI changes)

4. Request review from maintainers

### PR Review Process

1. **Automated checks** - CI must pass
2. **Code review** - At least {{MIN_REVIEWERS}} maintainer approval required
3. **Feedback** - Address any requested changes
4. **Merge** - Maintainer will merge when approved

### After Merge

- Delete your feature branch
- Update your local {{DEFAULT_BRANCH}}:
  ```bash
  git checkout {{DEFAULT_BRANCH}}
  git pull upstream {{DEFAULT_BRANCH}}
  ```

## Coding Standards

### Code Style

{{#STYLE_GUIDES}}
- {{STYLE_GUIDE}}
{{/STYLE_GUIDES}}

### Linting

We use {{LINTER_NAME}} for code linting:

```bash
# Check for issues
{{LINT_CHECK_COMMAND}}

# Auto-fix issues
{{LINT_FIX_COMMAND}}
```

### Type Checking

{{#HAS_TYPE_CHECKING}}
We use {{TYPE_CHECKER}} for type checking:

```bash
{{TYPE_CHECK_COMMAND}}
```
{{/HAS_TYPE_CHECKING}}

### File Organization

```
{{PROJECT_STRUCTURE}}
```

## Testing

### Running Tests

```bash
# Run all tests
{{TEST_COMMAND}}

# Run tests in watch mode
{{TEST_WATCH_COMMAND}}

# Run specific test file
{{TEST_FILE_COMMAND}}

# Run tests with coverage
{{TEST_COVERAGE_COMMAND}}
```

### Writing Tests

{{#TEST_GUIDELINES}}
- {{GUIDELINE}}
{{/TEST_GUIDELINES}}

### Test Structure

```{{CODE_LANGUAGE}}
{{TEST_EXAMPLE}}
```

### Coverage Requirements

- Minimum overall coverage: {{MIN_COVERAGE}}%
- New code should have at least {{NEW_CODE_COVERAGE}}% coverage

## Documentation

### When to Update Documentation

- Adding new features
- Changing existing behavior
- Adding new configuration options
- Fixing documentation bugs

### Documentation Style

{{#DOC_GUIDELINES}}
- {{GUIDELINE}}
{{/DOC_GUIDELINES}}

### Building Documentation

```bash
{{DOCS_BUILD_COMMAND}}
```

## Issue Reporting

### Bug Reports

When reporting bugs, include:

1. **Description** - Clear description of the bug
2. **Steps to Reproduce** - Step-by-step instructions
3. **Expected Behavior** - What should happen
4. **Actual Behavior** - What actually happens
5. **Environment** - OS, runtime version, etc.
6. **Screenshots** - If applicable

### Feature Requests

For feature requests, include:

1. **Problem** - What problem does this solve?
2. **Proposed Solution** - How should it work?
3. **Alternatives** - Other solutions considered
4. **Additional Context** - Any relevant information

## Questions?

- Check existing [Issues]({{ISSUES_URL}}) and [Discussions]({{DISCUSSIONS_URL}})
- Join our [{{COMMUNITY_PLATFORM}}]({{COMMUNITY_URL}})
- Contact maintainers at {{CONTACT_EMAIL}}

## Recognition

Contributors are recognized in:
- [CONTRIBUTORS.md](CONTRIBUTORS.md)
- Release notes

Thank you for contributing to {{PROJECT_NAME}}!
```

---

## Variable Reference

### Project Variables

| Variable | Type | Description |
|----------|------|-------------|
| `PROJECT_NAME` | string | Name of the project |
| `REPO_NAME` | string | Repository name |
| `REPOSITORY_URL` | string | Repository URL |
| `DEFAULT_BRANCH` | string | Default branch name (main/master) |
| `CONTACT_EMAIL` | string | Contact email for issues |

### Development Variables

| Variable | Type | Description |
|----------|------|-------------|
| `INSTALL_COMMAND` | string | Package install command |
| `TEST_COMMAND` | string | Test run command |
| `LINT_COMMAND` | string | Linting command |
| `LINT_FIX_COMMAND` | string | Lint fix command |
| `TYPE_CHECK_COMMAND` | string | Type checking command |

### Workflow Variables

| Variable | Type | Description |
|----------|------|-------------|
| `MIN_REVIEWERS` | number | Minimum required reviewers |
| `MIN_COVERAGE` | number | Minimum test coverage % |
| `NEW_CODE_COVERAGE` | number | Required coverage for new code |

### Documentation Variables

| Variable | Type | Description |
|----------|------|-------------|
| `DOCS_BUILD_COMMAND` | string | Documentation build command |
| `DOC_GUIDELINES` | array | Documentation guidelines |

### Community Variables

| Variable | Type | Description |
|----------|------|-------------|
| `ISSUES_URL` | string | Issues page URL |
| `DISCUSSIONS_URL` | string | Discussions page URL |
| `COMMUNITY_PLATFORM` | string | Community platform name |
| `COMMUNITY_URL` | string | Community link |

---

## Common Patterns by Language

### Node.js/TypeScript
```markdown
### Prerequisites
- Node.js (v18+)
- pnpm (v8+)

### Commands
\`\`\`bash
pnpm install          # Install dependencies
pnpm test             # Run tests
pnpm lint             # Check linting
pnpm lint:fix         # Fix linting issues
pnpm type-check       # TypeScript type checking
pnpm build            # Build project
\`\`\`
```

### Python
```markdown
### Prerequisites
- Python (3.10+)
- pip or poetry

### Commands
\`\`\`bash
pip install -e ".[dev]"   # Install with dev deps
pytest                    # Run tests
ruff check .              # Check linting
ruff format .             # Format code
mypy .                    # Type checking
\`\`\`
```

### Go
```markdown
### Prerequisites
- Go (1.21+)

### Commands
\`\`\`bash
go mod download       # Install dependencies
go test ./...         # Run tests
golangci-lint run     # Linting
go build ./...        # Build
\`\`\`
```

---

## Test Example Templates

### TypeScript (Jest)
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a user with valid input', async () => {
      const input = { email: 'test@example.com', name: 'Test' };
      const result = await userService.createUser(input);
      expect(result.email).toBe(input.email);
    });

    it('should throw error for duplicate email', async () => {
      await expect(userService.createUser(existingUser))
        .rejects.toThrow('Email already exists');
    });
  });
});
```

### Python (pytest)
```python
class TestUserService:
    def test_create_user_with_valid_input(self, user_service):
        input_data = {"email": "test@example.com", "name": "Test"}
        result = user_service.create_user(input_data)
        assert result.email == input_data["email"]

    def test_create_user_duplicate_email_raises(self, user_service):
        with pytest.raises(DuplicateEmailError):
            user_service.create_user(existing_user)
```
