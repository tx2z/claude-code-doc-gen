---
description: "[Internal] Setup guide template - use /doc-gen instead"
disable-model-invocation: true
---

# Setup Guide Template

Template for generating setup and installation documentation. Variables use mustache-style syntax: `{{VARIABLE_NAME}}`.

---

## Template

```markdown
# {{PROJECT_NAME}} Setup Guide

This guide will help you set up {{PROJECT_NAME}} for local development.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Detailed Setup](#detailed-setup)
- [Docker Setup](#docker-setup)
- [Troubleshooting](#troubleshooting)

## Prerequisites

### Required Software

| Software | Version | Installation |
|----------|---------|--------------|
{{#REQUIRED_SOFTWARE}}
| {{SOFTWARE_NAME}} | {{VERSION}} | [Download]({{DOWNLOAD_URL}}) |
{{/REQUIRED_SOFTWARE}}

### Optional Software

| Software | Version | Purpose |
|----------|---------|---------|
{{#OPTIONAL_SOFTWARE}}
| {{SOFTWARE_NAME}} | {{VERSION}} | {{PURPOSE}} |
{{/OPTIONAL_SOFTWARE}}

### Verify Installation

```bash
{{#VERIFY_COMMANDS}}
# Check {{SOFTWARE_NAME}}
{{VERIFY_COMMAND}}
{{/VERIFY_COMMANDS}}
```

{{#HAS_OS_INSTRUCTIONS}}
### Operating System Instructions

<details>
<summary>macOS</summary>

```bash
{{MACOS_INSTALL_COMMANDS}}
```
</details>

<details>
<summary>Windows</summary>

```powershell
{{WINDOWS_INSTALL_COMMANDS}}
```
</details>

<details>
<summary>Linux (Ubuntu/Debian)</summary>

```bash
{{LINUX_INSTALL_COMMANDS}}
```
</details>
{{/HAS_OS_INSTRUCTIONS}}

## Quick Start

For experienced developers who want to get up and running quickly:

```bash
{{QUICK_START_COMMANDS}}
```

The application will be available at: {{LOCAL_URL}}

## Detailed Setup

### 1. Clone the Repository

```bash
git clone {{REPOSITORY_URL}}
cd {{PROJECT_SLUG}}
```

### 2. Install Dependencies

```bash
{{INSTALL_COMMANDS}}
```

{{#HAS_POST_INSTALL}}
The following scripts run automatically after installation:
{{#POST_INSTALL_SCRIPTS}}
- `{{SCRIPT_NAME}}` - {{SCRIPT_PURPOSE}}
{{/POST_INSTALL_SCRIPTS}}
{{/HAS_POST_INSTALL}}

### 3. Environment Configuration

Copy the environment template:

```bash
cp .env.example .env
```

Configure the following variables in `.env`:

#### Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
{{#REQUIRED_ENV_VARS}}
| `{{VAR_NAME}}` | {{VAR_DESCRIPTION}} | `{{VAR_EXAMPLE}}` |
{{/REQUIRED_ENV_VARS}}

#### Optional Variables

| Variable | Description | Default |
|----------|-------------|---------|
{{#OPTIONAL_ENV_VARS}}
| `{{VAR_NAME}}` | {{VAR_DESCRIPTION}} | `{{VAR_DEFAULT}}` |
{{/OPTIONAL_ENV_VARS}}

{{#HAS_SECRET_GENERATION}}
#### Generating Secrets

```bash
{{#SECRET_COMMANDS}}
# Generate {{SECRET_NAME}}
{{SECRET_COMMAND}}
{{/SECRET_COMMANDS}}
```
{{/HAS_SECRET_GENERATION}}

### 4. Database Setup

{{#USE_DOCKER_DB}}
#### Using Docker (Recommended)

```bash
# Start database containers
docker compose up -d {{DB_SERVICE_NAMES}}

# Verify containers are running
docker compose ps
```
{{/USE_DOCKER_DB}}

{{#USE_LOCAL_DB}}
#### Local Installation

{{#DB_SETUP_INSTRUCTIONS}}
**{{DB_NAME}}:**

```bash
{{DB_SETUP_COMMANDS}}
```
{{/DB_SETUP_INSTRUCTIONS}}
{{/USE_LOCAL_DB}}

#### Run Migrations

```bash
{{MIGRATION_COMMAND}}
```

{{#HAS_SEED}}
#### Seed Database (Optional)

```bash
{{SEED_COMMAND}}
```
{{/HAS_SEED}}

{{#HAS_THIRD_PARTY_SERVICES}}
### 5. Third-Party Services

{{#THIRD_PARTY_SERVICES}}
#### {{SERVICE_NAME}}

1. {{SETUP_STEP_1}}
2. {{SETUP_STEP_2}}
3. Add to `.env`:
   ```
   {{ENV_VARS}}
   ```

{{#HAS_LOCAL_ALTERNATIVE}}
**Local Alternative:**
```bash
{{LOCAL_ALTERNATIVE_COMMAND}}
```
{{/HAS_LOCAL_ALTERNATIVE}}

{{/THIRD_PARTY_SERVICES}}
{{/HAS_THIRD_PARTY_SERVICES}}

### {{FINAL_STEP_NUMBER}}. Start Development Server

```bash
{{DEV_COMMAND}}
```

The application will be available at:
{{#APP_URLS}}
- **{{URL_NAME}}:** {{URL}}
{{/APP_URLS}}

## Available Commands

| Command | Description |
|---------|-------------|
{{#AVAILABLE_COMMANDS}}
| `{{COMMAND}}` | {{DESCRIPTION}} |
{{/AVAILABLE_COMMANDS}}

## Docker Setup

{{#HAS_DOCKER}}
### Development with Docker Compose

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

### Docker Services

| Service | Port | Description |
|---------|------|-------------|
{{#DOCKER_SERVICES}}
| `{{SERVICE_NAME}}` | {{PORT}} | {{DESCRIPTION}} |
{{/DOCKER_SERVICES}}

### Building Docker Images

```bash
# Build all images
docker compose build

# Build with no cache
docker compose build --no-cache
```

{{#HAS_PROD_DOCKER}}
### Production Docker Build

```bash
# Build production image
docker build -t {{DOCKER_IMAGE_NAME}}:latest .

# Run production container
docker run -p {{PROD_PORT}}:{{CONTAINER_PORT}} \\
  -e NODE_ENV=production \\
  {{DOCKER_IMAGE_NAME}}:latest
```
{{/HAS_PROD_DOCKER}}
{{/HAS_DOCKER}}

{{#HAS_IDE_SETUP}}
## IDE Setup

### VS Code

Recommended extensions:
{{#VSCODE_EXTENSIONS}}
- {{EXTENSION_NAME}} (`{{EXTENSION_ID}}`)
{{/VSCODE_EXTENSIONS}}

<details>
<summary>settings.json</summary>

```json
{{VSCODE_SETTINGS}}
```
</details>

{{#HAS_DEBUGGING}}
### Debugging

Add to `.vscode/launch.json`:

```json
{{LAUNCH_CONFIG}}
```

Start debugging:
```bash
{{DEBUG_COMMAND}}
```
{{/HAS_DEBUGGING}}
{{/HAS_IDE_SETUP}}

## Troubleshooting

### Common Issues

{{#TROUBLESHOOTING_ISSUES}}
#### {{ISSUE_TITLE}}

**Problem:** {{ISSUE_DESCRIPTION}}

**Solution:**
```bash
{{ISSUE_SOLUTION}}
```
{{/TROUBLESHOOTING_ISSUES}}

### Getting Help

If you encounter issues not covered here:

1. Check the [GitHub Issues]({{ISSUES_URL}})
2. Search existing issues for similar problems
3. Create a new issue with:
   - {{RUNTIME_NAME}} version (`{{VERSION_CHECK_COMMAND}}`)
   - Operating system
   - Error message (full stack trace)
   - Steps to reproduce

## Next Steps

After completing setup:

{{#NEXT_STEPS}}
- [ ] {{STEP}}
{{/NEXT_STEPS}}

## Additional Resources

{{#RESOURCES}}
- [{{RESOURCE_NAME}}]({{RESOURCE_URL}})
{{/RESOURCES}}
```

---

## Variable Reference

### Project Variables

| Variable | Type | Description |
|----------|------|-------------|
| `PROJECT_NAME` | string | Name of the project |
| `PROJECT_SLUG` | string | Directory name (lowercase, dashes) |
| `REPOSITORY_URL` | string | Git clone URL |
| `LOCAL_URL` | string | Local development URL |

### Software Variables

| Variable | Type | Description |
|----------|------|-------------|
| `REQUIRED_SOFTWARE` | array | Required software list |
| `OPTIONAL_SOFTWARE` | array | Optional software list |
| `SOFTWARE_NAME` | string | Name of software |
| `VERSION` | string | Required version |
| `DOWNLOAD_URL` | string | Download link |

### Environment Variables

| Variable | Type | Description |
|----------|------|-------------|
| `REQUIRED_ENV_VARS` | array | Required environment variables |
| `OPTIONAL_ENV_VARS` | array | Optional environment variables |
| `VAR_NAME` | string | Variable name |
| `VAR_DESCRIPTION` | string | What the variable is for |
| `VAR_EXAMPLE` | string | Example value |
| `VAR_DEFAULT` | string | Default value |

### Command Variables

| Variable | Type | Description |
|----------|------|-------------|
| `INSTALL_COMMANDS` | string | Package install commands |
| `DEV_COMMAND` | string | Development server command |
| `MIGRATION_COMMAND` | string | Database migration command |
| `SEED_COMMAND` | string | Database seed command |

### Docker Variables

| Variable | Type | Description |
|----------|------|-------------|
| `DOCKER_SERVICES` | array | Docker compose services |
| `DOCKER_IMAGE_NAME` | string | Docker image name |
| `DB_SERVICE_NAMES` | string | Database service names |

### Troubleshooting Variables

| Variable | Type | Description |
|----------|------|-------------|
| `TROUBLESHOOTING_ISSUES` | array | Common issues |
| `ISSUE_TITLE` | string | Issue name |
| `ISSUE_DESCRIPTION` | string | Problem description |
| `ISSUE_SOLUTION` | string | Solution commands |

---

## Common Setup Patterns

### Node.js Project
```bash
# Quick Start
git clone {{REPOSITORY_URL}}
cd {{PROJECT_SLUG}}
pnpm install
cp .env.example .env
docker compose up -d postgres redis
pnpm db:migrate
pnpm dev
```

### Python Project
```bash
# Quick Start
git clone {{REPOSITORY_URL}}
cd {{PROJECT_SLUG}}
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
docker compose up -d postgres redis
python manage.py migrate
python manage.py runserver
```

### Go Project
```bash
# Quick Start
git clone {{REPOSITORY_URL}}
cd {{PROJECT_SLUG}}
go mod download
cp .env.example .env
docker compose up -d postgres redis
go run cmd/migrate/main.go
go run cmd/server/main.go
```

---

## Troubleshooting Templates

### Port Conflict
```markdown
#### Port Already in Use

**Problem:** Error: listen EADDRINUSE: address already in use :::3000

**Solution:**
\`\`\`bash
# Find process using port
lsof -i :3000

# Kill process
kill -9 <PID>

# Or use different port
PORT=3001 pnpm dev
\`\`\`
```

### Node Version
```markdown
#### Node Version Mismatch

**Problem:** Error: The engine "node" is incompatible

**Solution:**
\`\`\`bash
# Install correct version with nvm
nvm install {{NODE_VERSION}}
nvm use {{NODE_VERSION}}

# Set as default
nvm alias default {{NODE_VERSION}}
\`\`\`
```

### Docker Permission
```markdown
#### Permission Denied (Docker)

**Problem:** Got permission denied while trying to connect to Docker daemon

**Solution:**
\`\`\`bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER

# Log out and back in, or:
newgrp docker
\`\`\`
```
