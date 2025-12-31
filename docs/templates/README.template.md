---
description: "[Internal] README template - use /doc-gen instead"
disable-model-invocation: true
---

# README Template

Template for generating project README.md files. Variables use mustache-style syntax: `{{VARIABLE_NAME}}`.

---

## Template

```markdown
# {{PROJECT_NAME}}

{{DESCRIPTION}}

{{BADGES}}

## Features

{{#FEATURES}}
- **{{FEATURE_NAME}}** - {{FEATURE_DESCRIPTION}}
{{/FEATURES}}

## Tech Stack

{{#TECH_STACK_TABLE}}
| Category | Technology |
|----------|------------|
{{#TECH_ITEMS}}
| {{CATEGORY}} | {{TECHNOLOGY}} |
{{/TECH_ITEMS}}
{{/TECH_STACK_TABLE}}

{{#TECH_STACK_LIST}}
{{#TECH_ITEMS}}
- [{{TECHNOLOGY}}]({{TECH_URL}}) - {{TECH_DESCRIPTION}}
{{/TECH_ITEMS}}
{{/TECH_STACK_LIST}}

## Prerequisites

Before you begin, ensure you have the following installed:

{{#PREREQUISITES}}
- [{{PREREQ_NAME}}]({{PREREQ_URL}}) ({{PREREQ_VERSION}})
{{/PREREQUISITES}}

## Installation

{{#IS_PACKAGE}}
```bash
# npm
npm install {{PACKAGE_NAME}}

# pnpm
pnpm add {{PACKAGE_NAME}}

# yarn
yarn add {{PACKAGE_NAME}}
```
{{/IS_PACKAGE}}

{{#IS_PROJECT}}
```bash
# Clone the repository
git clone {{REPOSITORY_URL}}
cd {{PROJECT_SLUG}}

# Install dependencies
{{INSTALL_COMMAND}}

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Start the development server
{{DEV_COMMAND}}
```
{{/IS_PROJECT}}

## Quick Start

```{{CODE_LANGUAGE}}
{{QUICK_START_CODE}}
```

{{#HAS_CONFIGURATION}}
## Configuration

{{#HAS_ENV_TABLE}}
| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
{{#ENV_VARS}}
| `{{VAR_NAME}}` | {{VAR_DESCRIPTION}} | {{VAR_REQUIRED}} | {{VAR_DEFAULT}} |
{{/ENV_VARS}}
{{/HAS_ENV_TABLE}}

{{#HAS_ENV_EXAMPLE}}
Example `.env` file:

```env
{{ENV_EXAMPLE}}
```
{{/HAS_ENV_EXAMPLE}}
{{/HAS_CONFIGURATION}}

## Usage

{{#USAGE_SECTIONS}}
### {{USAGE_TITLE}}

```{{CODE_LANGUAGE}}
{{USAGE_CODE}}
```
{{/USAGE_SECTIONS}}

{{#HAS_SCRIPTS}}
## Available Scripts

| Script | Description |
|--------|-------------|
{{#SCRIPTS}}
| `{{SCRIPT_COMMAND}}` | {{SCRIPT_DESCRIPTION}} |
{{/SCRIPTS}}
{{/HAS_SCRIPTS}}

{{#HAS_PROJECT_STRUCTURE}}
## Project Structure

```
{{PROJECT_NAME}}/
{{PROJECT_TREE}}
```
{{/HAS_PROJECT_STRUCTURE}}

{{#HAS_API_DOCS}}
## API Reference

See the [API documentation]({{API_DOCS_URL}}) for detailed endpoint information.
{{/HAS_API_DOCS}}

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/{{FEATURE_BRANCH}}`)
3. Commit your changes (`git commit -m 'Add {{FEATURE_NAME}}'`)
4. Push to the branch (`git push origin feature/{{FEATURE_BRANCH}}`)
5. Open a Pull Request

## License

This project is licensed under the {{LICENSE_NAME}} License - see the [LICENSE](LICENSE) file for details.

{{#HAS_LINKS}}
## Links

{{#LINKS}}
- [{{LINK_NAME}}]({{LINK_URL}})
{{/LINKS}}
{{/HAS_LINKS}}

{{#HAS_ACKNOWLEDGMENTS}}
## Acknowledgments

{{#ACKNOWLEDGMENTS}}
- {{ACKNOWLEDGMENT}}
{{/ACKNOWLEDGMENTS}}
{{/HAS_ACKNOWLEDGMENTS}}
```

---

## Variable Reference

### Required Variables

| Variable | Type | Description |
|----------|------|-------------|
| `PROJECT_NAME` | string | Project name (title case) |
| `DESCRIPTION` | string | One-line project description |
| `LICENSE_NAME` | string | License name (MIT, Apache 2.0, etc.) |

### Optional Variables

| Variable | Type | Description |
|----------|------|-------------|
| `BADGES` | string | Badge images (build, coverage, npm, etc.) |
| `REPOSITORY_URL` | string | Git clone URL |
| `PACKAGE_NAME` | string | npm/PyPI package name |
| `INSTALL_COMMAND` | string | Package manager install command |
| `DEV_COMMAND` | string | Development server command |
| `CODE_LANGUAGE` | string | Primary language (typescript, python, etc.) |

### Conditional Sections

| Section | Condition | Description |
|---------|-----------|-------------|
| `IS_PACKAGE` | Package published to registry | Shows package install commands |
| `IS_PROJECT` | Standalone project | Shows clone and setup steps |
| `HAS_CONFIGURATION` | Has env vars | Shows configuration section |
| `HAS_SCRIPTS` | Has package scripts | Shows scripts table |
| `HAS_PROJECT_STRUCTURE` | Complex structure | Shows directory tree |
| `HAS_API_DOCS` | Has API | Links to API docs |

### Collection Variables

| Variable | Item Fields | Description |
|----------|-------------|-------------|
| `FEATURES` | `FEATURE_NAME`, `FEATURE_DESCRIPTION` | Feature list |
| `TECH_ITEMS` | `CATEGORY`, `TECHNOLOGY`, `TECH_URL`, `TECH_DESCRIPTION` | Tech stack |
| `PREREQUISITES` | `PREREQ_NAME`, `PREREQ_URL`, `PREREQ_VERSION` | Required software |
| `ENV_VARS` | `VAR_NAME`, `VAR_DESCRIPTION`, `VAR_REQUIRED`, `VAR_DEFAULT` | Environment variables |
| `SCRIPTS` | `SCRIPT_COMMAND`, `SCRIPT_DESCRIPTION` | Available scripts |
| `LINKS` | `LINK_NAME`, `LINK_URL` | Related links |

---

## Badge Templates

### Build Status
```markdown
![Build Status](https://github.com/{{OWNER}}/{{REPO}}/workflows/CI/badge.svg)
```

### npm Version
```markdown
[![npm version](https://img.shields.io/npm/v/{{PACKAGE_NAME}}.svg)](https://www.npmjs.com/package/{{PACKAGE_NAME}})
```

### Downloads
```markdown
[![npm downloads](https://img.shields.io/npm/dm/{{PACKAGE_NAME}}.svg)](https://www.npmjs.com/package/{{PACKAGE_NAME}})
```

### License
```markdown
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
```

### TypeScript
```markdown
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)](https://www.typescriptlang.org/)
```

### Code Coverage
```markdown
[![codecov](https://codecov.io/gh/{{OWNER}}/{{REPO}}/branch/main/graph/badge.svg)](https://codecov.io/gh/{{OWNER}}/{{REPO}})
```

---

## Example Output

```markdown
# My Awesome Project

A modern web application for managing tasks with real-time collaboration.

![Build Status](https://github.com/user/my-project/workflows/CI/badge.svg)
[![npm version](https://img.shields.io/npm/v/my-project.svg)](https://www.npmjs.com/package/my-project)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Features

- **Real-time Collaboration** - Work together with your team in real-time
- **Task Management** - Create, organize, and track tasks efficiently
- **Notifications** - Get notified about important updates
- **Dark Mode** - Easy on the eyes with dark theme support

## Tech Stack

| Category | Technology |
|----------|------------|
| Frontend | React 18 |
| Backend | NestJS |
| Database | PostgreSQL |
| Cache | Redis |

## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v18.0.0+)
- [pnpm](https://pnpm.io/) (v8.0.0+)
- [Docker](https://www.docker.com/) (v20.10+)

## Installation

\`\`\`bash
# Clone the repository
git clone https://github.com/user/my-project.git
cd my-project

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example .env

# Start the development server
pnpm dev
\`\`\`

## Quick Start

\`\`\`typescript
import { TaskManager } from 'my-project';

const manager = new TaskManager();
await manager.createTask({
  title: 'My First Task',
  priority: 'high',
});
\`\`\`

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
