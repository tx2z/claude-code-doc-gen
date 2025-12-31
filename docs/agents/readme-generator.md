---
description: "[Internal] README generator agent - use /doc-gen instead"
disable-model-invocation: true
allowed-tools: Read, Glob, Grep
---

# README Generator Agent (DOC01)

Generate comprehensive, professional README.md files that serve as the primary entry point for understanding a project.

---

## 1. Information Gathering

### 1.1 Project Identity

**Search for project name and metadata:**
```
Glob: package.json, pyproject.toml, setup.py, Cargo.toml, go.mod, *.csproj, pom.xml, composer.json
```

**Extract:**
- [ ] Project name (use directory name as fallback)
- [ ] Version number
- [ ] Description/summary
- [ ] Keywords/tags
- [ ] Author/maintainers
- [ ] License type
- [ ] Repository URL
- [ ] Homepage URL

**Node.js Example:**
```json
{
  "name": "awesome-project",
  "version": "1.0.0",
  "description": "An awesome project",
  "author": "Jane Doe",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/user/awesome-project"
  }
}
```

### 1.2 Project Purpose Analysis

**Search for clues about project purpose:**
```
Glob: README*, ABOUT*, DESCRIPTION*
Grep: "This project"|"This package"|"This library"|"Purpose"|"Goal"
```

**Analyze source code to determine:**
- [ ] Is it a library/package? (exports functions/classes)
- [ ] Is it a CLI tool? (bin field, commander, yargs)
- [ ] Is it a web app? (express, react, vue routes)
- [ ] Is it an API service? (controllers, endpoints)
- [ ] Is it a utility/tool? (scripts, automation)

**Look for entry points:**
```
Glob: src/index.*, src/main.*, lib/index.*, app.*, main.*
```

### 1.3 Features Extraction

**Search for feature indicators:**
```
Glob: src/**/*.ts, src/**/*.js, src/**/*.py, lib/**/*
Grep: export (function|class|const)|def |@app\.route|@Controller|@Component
```

**Identify key features by analyzing:**
- [ ] Exported functions/classes (public API)
- [ ] Route definitions (endpoints)
- [ ] Components (UI features)
- [ ] Services (business logic)
- [ ] Utilities (helper functions)

**Extract feature descriptions from:**
- JSDoc/docstring comments
- Function/class names (convert to human-readable)
- Test descriptions

### 1.4 Tech Stack Detection

**Analyze dependencies:**
```
Glob: package.json, requirements.txt, Pipfile, go.mod, Cargo.toml, *.csproj
```

**Categorize dependencies:**

**Runtime Dependencies:**
- Core framework (Express, React, Django, etc.)
- Database clients (PostgreSQL, MongoDB, Redis)
- Authentication (Passport, JWT, OAuth)
- API clients (Axios, fetch wrappers)

**Development Dependencies:**
- Testing (Jest, Pytest, Go test)
- Linting (ESLint, Pylint, golangci-lint)
- Building (Webpack, Vite, tsc)
- Type checking (TypeScript, mypy)

### 1.5 Configuration Analysis

**Search for configuration options:**
```
Glob: .env.example, .env.sample, config/*.*, src/config/*.*
Grep: process\.env\.|os\.environ|getenv|Config\(|@Value
```

**Document each configuration option:**
- [ ] Name (ENV_VAR_NAME)
- [ ] Description
- [ ] Required/optional
- [ ] Default value
- [ ] Example value

### 1.6 Scripts/Commands Analysis

**For Node.js projects:**
```
Grep: "scripts"
Glob: package.json
```

**Document each script:**
- [ ] Script name
- [ ] What it does
- [ ] When to use it

**For Python projects:**
```
Grep: entry_points|scripts
Glob: setup.py, pyproject.toml
```

**For other projects:**
```
Glob: Makefile, justfile, taskfile.yml
Grep: .PHONY|^[a-z-]+:
```

### 1.7 Project Structure Analysis

**Map directory structure:**
```
ls -la
ls src/ || ls lib/ || ls app/
ls tests/ || ls test/
ls docs/
```

**Identify key directories:**
- Source code location
- Tests location
- Documentation location
- Configuration location
- Assets/static files
- Build output

---

## 2. Badge Generation

### 2.1 Standard Badges

**Determine applicable badges based on project type:**

**Version/Release:**
```markdown
![npm version](https://img.shields.io/npm/v/{{PACKAGE_NAME}})
![PyPI version](https://img.shields.io/pypi/v/{{PACKAGE_NAME}})
![Crates.io version](https://img.shields.io/crates/v/{{PACKAGE_NAME}})
```

**Build Status:**
```markdown
![GitHub Actions](https://github.com/{{OWNER}}/{{REPO}}/workflows/CI/badge.svg)
![Travis CI](https://img.shields.io/travis/{{OWNER}}/{{REPO}})
![CircleCI](https://img.shields.io/circleci/build/github/{{OWNER}}/{{REPO}})
```

**Code Quality:**
```markdown
![Codecov](https://codecov.io/gh/{{OWNER}}/{{REPO}}/branch/main/graph/badge.svg)
![Code Climate](https://img.shields.io/codeclimate/maintainability/{{OWNER}}/{{REPO}})
![Snyk](https://snyk.io/test/github/{{OWNER}}/{{REPO}}/badge.svg)
```

**License:**
```markdown
![License](https://img.shields.io/github/license/{{OWNER}}/{{REPO}})
![License](https://img.shields.io/npm/l/{{PACKAGE_NAME}})
```

**Downloads/Usage:**
```markdown
![npm downloads](https://img.shields.io/npm/dm/{{PACKAGE_NAME}})
![PyPI downloads](https://img.shields.io/pypi/dm/{{PACKAGE_NAME}})
```

### 2.2 Badge Selection Logic

**Include based on detection:**
- [ ] If package.json → npm badges
- [ ] If pyproject.toml → PyPI badges
- [ ] If .github/workflows → GitHub Actions badge
- [ ] If codecov.yml → Codecov badge
- [ ] If LICENSE file → License badge

---

## 3. Section Generation

### 3.1 Title and Description

**Format:**
```markdown
# {{PROJECT_NAME}}

{{ONE_LINE_DESCRIPTION}}

{{BADGES}}
```

**Guidelines:**
- Project name should match package name
- Description should be 1-2 sentences max
- Badges on single line, separated by spaces

### 3.2 Features Section

**Format:**
```markdown
## Features

- **Feature Name** - Brief description of what it does
- **Another Feature** - Brief description
- **Third Feature** - Brief description
```

**Extraction patterns:**
- Look for exported functions/classes
- Analyze public API
- Check test descriptions for feature names

**Example (from React component library):**
```markdown
## Features

- **Button** - Customizable button with variants and sizes
- **Modal** - Accessible modal dialog with animations
- **Form** - Form components with validation
- **Toast** - Toast notifications with auto-dismiss
```

### 3.3 Tech Stack Section

**Format (for complex projects):**
```markdown
## Tech Stack

| Category | Technology |
|----------|------------|
| Language | TypeScript |
| Framework | NestJS |
| Database | PostgreSQL |
| Cache | Redis |
| Testing | Jest |
```

**Or simple format:**
```markdown
## Built With

- [TypeScript](https://www.typescriptlang.org/) - Type-safe JavaScript
- [NestJS](https://nestjs.com/) - Progressive Node.js framework
- [PostgreSQL](https://www.postgresql.org/) - Relational database
```

### 3.4 Prerequisites Section

**Format:**
```markdown
## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v18 or higher)
- [npm](https://www.npmjs.com/) or [pnpm](https://pnpm.io/)
- [Docker](https://www.docker.com/) (optional, for containers)
```

**Detection:**
```
Grep: "engines"|"node"|"python"|"requires-python"
Glob: package.json, pyproject.toml, .nvmrc, .python-version
```

### 3.5 Installation Section

**Format by language:**

**Node.js:**
```markdown
## Installation

```bash
# Clone the repository
git clone https://github.com/{{OWNER}}/{{REPO}}.git
cd {{REPO}}

# Install dependencies
npm install
# or
pnpm install

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Start the development server
npm run dev
```
```

**Python:**
```markdown
## Installation

```bash
# Clone the repository
git clone https://github.com/{{OWNER}}/{{REPO}}.git
cd {{REPO}}

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
# or for development
pip install -e ".[dev]"

# Set up environment variables
cp .env.example .env

# Run the application
python main.py
```
```

**Package installation (for libraries):**
```markdown
## Installation

```bash
# npm
npm install {{PACKAGE_NAME}}

# pnpm
pnpm add {{PACKAGE_NAME}}

# yarn
yarn add {{PACKAGE_NAME}}
```

Or for Python:
```bash
pip install {{PACKAGE_NAME}}
```
```

### 3.6 Quick Start Section

**Format:**
```markdown
## Quick Start

```typescript
import { createClient } from '{{PACKAGE_NAME}}';

// Initialize the client
const client = createClient({
  apiKey: 'your-api-key',
});

// Use the main feature
const result = await client.doSomething({
  input: 'example',
});

console.log(result);
```
```

**Guidelines:**
- Show the most common use case
- Keep code to 10-15 lines max
- Include imports
- Use realistic but simple examples
- Add comments explaining each step

### 3.7 Configuration Section

**Format:**
```markdown
## Configuration

Create a `.env` file in the root directory:

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `DATABASE_URL` | PostgreSQL connection string | Yes | - |
| `JWT_SECRET` | Secret for JWT tokens | Yes | - |
| `PORT` | Server port | No | `3000` |
| `LOG_LEVEL` | Logging level | No | `info` |

Example `.env` file:
```env
DATABASE_URL=postgresql://user:pass@localhost:5432/mydb
JWT_SECRET=your-secret-key
PORT=3000
LOG_LEVEL=debug
```
```

### 3.8 Usage Examples Section

**Format:**
```markdown
## Usage

### Basic Usage

```typescript
// Basic example with minimal configuration
```

### Advanced Usage

```typescript
// More complex example with all options
```

### API Example

```typescript
// Example making API calls
```
```

**Guidelines:**
- Start with simplest example
- Progress to more complex scenarios
- Cover common use cases
- Include error handling in at least one example

### 3.9 Scripts Reference Section

**Format (Node.js):**
```markdown
## Available Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build for production |
| `npm run test` | Run test suite |
| `npm run lint` | Lint and fix code style |
| `npm run type-check` | Run TypeScript type checking |
```

### 3.10 Project Structure Section

**Format:**
```markdown
## Project Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── components/     # React components
│   ├── hooks/          # Custom React hooks
│   ├── services/       # API services
│   ├── utils/          # Utility functions
│   └── index.ts        # Entry point
├── tests/              # Test files
├── docs/               # Documentation
├── .github/            # GitHub workflows
├── package.json        # Dependencies
└── README.md           # This file
```
```

**Guidelines:**
- Show only important directories (2-3 levels deep)
- Add brief comments explaining each
- Focus on source code structure

### 3.11 Contributing Section

**Format:**
```markdown
## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
```

### 3.12 License Section

**Format:**
```markdown
## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

### 3.13 Links Section (optional)

**Format:**
```markdown
## Links

- [Documentation](https://{{PROJECT_NAME}}.docs.example.com)
- [API Reference](./docs/api/)
- [Changelog](./CHANGELOG.md)
- [Contributing](./CONTRIBUTING.md)
- [Report a Bug](https://github.com/{{OWNER}}/{{REPO}}/issues)
```

---

## 4. Language-Specific Patterns

### 4.1 TypeScript/JavaScript

**README characteristics:**
- Include npm badges
- Show TypeScript/JavaScript usage examples
- Reference type definitions
- Include ESM and CommonJS examples if applicable

**Example import section:**
```markdown
### Import

```typescript
// ESM
import { something } from '{{PACKAGE_NAME}}';

// CommonJS
const { something } = require('{{PACKAGE_NAME}}');
```
```

### 4.2 Python

**README characteristics:**
- Include PyPI badges
- Show Python version compatibility
- Reference type hints
- Include pip and poetry installation

**Example:**
```markdown
### Requirements

- Python 3.8+
- pip or poetry

### Installation

```bash
pip install {{PACKAGE_NAME}}
# or with poetry
poetry add {{PACKAGE_NAME}}
```
```

### 4.3 Go

**README characteristics:**
- Include Go Report Card badge
- Show Go module import path
- Reference godoc

**Example:**
```markdown
### Installation

```bash
go get github.com/{{OWNER}}/{{REPO}}
```
```

### 4.4 Rust

**README characteristics:**
- Include crates.io badges
- Show Cargo.toml dependency
- Reference docs.rs

**Example:**
```markdown
### Installation

Add to your `Cargo.toml`:

```toml
[dependencies]
{{PACKAGE_NAME}} = "1.0"
```
```

---

## 5. Output Format

Generate README following this structure:

```markdown
# {{PROJECT_NAME}}

{{DESCRIPTION}}

{{BADGES}}

## Features

{{FEATURES_LIST}}

## Tech Stack

{{TECH_STACK}}

## Prerequisites

{{PREREQUISITES}}

## Installation

{{INSTALLATION_STEPS}}

## Quick Start

{{QUICK_START_CODE}}

## Configuration

{{CONFIGURATION_OPTIONS}}

## Usage

{{USAGE_EXAMPLES}}

## Available Scripts

{{SCRIPTS_TABLE}}

## Project Structure

{{DIRECTORY_TREE}}

## Contributing

{{CONTRIBUTING_INFO}}

## License

{{LICENSE_INFO}}

## Links

{{LINKS}}
```

---

## 6. Quality Checks

Before finalizing, verify:

- [ ] All links are valid and relative where appropriate
- [ ] Code examples are syntactically correct
- [ ] No placeholder text remains ({{VARIABLE}})
- [ ] Badges render correctly
- [ ] Installation steps are tested/verified
- [ ] Project name is consistent throughout
- [ ] No duplicate sections
- [ ] Proper markdown formatting
- [ ] Appropriate heading levels (h1 for title, h2 for sections)
