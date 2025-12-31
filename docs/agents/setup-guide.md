---
description: "[Internal] Setup guide agent - use /doc-gen instead"
disable-model-invocation: true
allowed-tools: Read, Glob, Grep
---

# Setup Guide Agent (DOC04)

Generate comprehensive setup and installation documentation that enables developers to get started quickly with a project.

---

## 1. Prerequisites Analysis

### 1.1 Runtime Requirements

**Detect Node.js requirements:**
```
Glob: package.json, .nvmrc, .node-version
Grep: "engines"|"node"
```

**Node.js version detection:**
```json
{
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=9.0.0"
  }
}
```

**Detect Python requirements:**
```
Glob: pyproject.toml, setup.py, .python-version, runtime.txt
Grep: requires-python|python_requires
```

**Detect other runtimes:**
```
Glob: go.mod, Cargo.toml, *.csproj, pom.xml
Grep: go |rust-version|TargetFramework|java.version
```

### 1.2 Package Manager Detection

**Node.js package managers:**
```
Glob: package-lock.json, pnpm-lock.yaml, yarn.lock, bun.lockb
```

| File | Package Manager | Install Command |
|------|-----------------|-----------------|
| `package-lock.json` | npm | `npm install` |
| `pnpm-lock.yaml` | pnpm | `pnpm install` |
| `yarn.lock` | yarn | `yarn install` |
| `bun.lockb` | bun | `bun install` |

**Python package managers:**
```
Glob: requirements.txt, Pipfile, pyproject.toml, poetry.lock
```

### 1.3 External Service Dependencies

**Database detection:**
```
Grep: postgres|mysql|mongodb|redis|sqlite|mariadb
Glob: package.json, requirements.txt, docker-compose.yml
```

**Search for connection strings:**
```
Grep: DATABASE_URL|MONGO_URI|REDIS_URL|DB_HOST
Glob: .env.example, .env.sample, config/*.ts
```

**Common services to detect:**
- [ ] PostgreSQL
- [ ] MySQL/MariaDB
- [ ] MongoDB
- [ ] Redis
- [ ] Elasticsearch
- [ ] RabbitMQ/Kafka
- [ ] MinIO/S3

### 1.4 Third-Party API Dependencies

**Search for API integrations:**
```
Grep: STRIPE_|TWILIO_|SENDGRID_|AWS_|GOOGLE_|GITHUB_
Glob: .env.example, .env.sample
```

---

## 2. Prerequisites Documentation

### 2.1 Required Software Section

```markdown
## Prerequisites

Before you begin, ensure you have the following installed:

### Required

| Software | Version | Installation |
|----------|---------|--------------|
| Node.js | v18.0.0+ | [Download](https://nodejs.org/) or use [nvm](https://github.com/nvm-sh/nvm) |
| pnpm | v8.0.0+ | `npm install -g pnpm` |
| Git | 2.0+ | [Download](https://git-scm.com/) |

### Optional (for local development)

| Software | Version | Purpose |
|----------|---------|---------|
| Docker | 20.10+ | Run services locally |
| Docker Compose | 2.0+ | Multi-container setup |
| PostgreSQL | 15+ | If not using Docker |
| Redis | 7+ | If not using Docker |

### Verify Installation

```bash
# Check Node.js version
node --version  # Should output v18.x.x or higher

# Check pnpm version
pnpm --version  # Should output 8.x.x or higher

# Check Docker (optional)
docker --version
docker compose version
```
```

### 2.2 System-Specific Instructions

**Generate OS-specific sections:**

```markdown
### macOS

```bash
# Install Homebrew (if not installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Node.js via nvm
brew install nvm
nvm install 18
nvm use 18

# Install pnpm
brew install pnpm

# Install Docker Desktop
brew install --cask docker
```

### Windows

```powershell
# Install via winget
winget install OpenJS.NodeJS.LTS
winget install pnpm.pnpm
winget install Docker.DockerDesktop

# Or use Chocolatey
choco install nodejs-lts pnpm docker-desktop
```

### Linux (Ubuntu/Debian)

```bash
# Install Node.js via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18

# Install pnpm
curl -fsSL https://get.pnpm.io/install.sh | sh -

# Install Docker
sudo apt-get update
sudo apt-get install docker.io docker-compose-plugin
sudo usermod -aG docker $USER
```
```

---

## 3. Environment Setup

### 3.1 Analyze Environment Variables

**Search for environment configuration:**
```
Glob: .env.example, .env.sample, .env.template, .env.development
Grep: ^[A-Z_]+=
```

**Categorize environment variables:**

**Required (no default, critical):**
- Database connection strings
- API keys for external services
- JWT secrets

**Optional (has defaults):**
- Port numbers
- Log levels
- Feature flags

### 3.2 Environment Configuration Documentation

```markdown
## Environment Setup

### 1. Copy Environment Template

```bash
cp .env.example .env
```

### 2. Configure Required Variables

Open `.env` and configure the following:

#### Database Configuration

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@localhost:5432/mydb` |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379` |

#### Authentication

| Variable | Description | How to Generate |
|----------|-------------|-----------------|
| `JWT_SECRET` | Secret for signing JWT tokens | `openssl rand -base64 32` |
| `JWT_REFRESH_SECRET` | Secret for refresh tokens | `openssl rand -base64 32` |

#### External Services

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `STRIPE_SECRET_KEY` | Stripe API key | [Stripe Dashboard](https://dashboard.stripe.com/apikeys) |
| `SENDGRID_API_KEY` | SendGrid API key | [SendGrid Settings](https://app.sendgrid.com/settings/api_keys) |

### 3. Optional Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `3000` | Server port |
| `LOG_LEVEL` | `info` | Logging level (debug, info, warn, error) |
| `NODE_ENV` | `development` | Environment mode |

### Example Complete .env

```env
# Database
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/myapp_dev
REDIS_URL=redis://localhost:6379

# Authentication
JWT_SECRET=your-super-secret-jwt-key-min-32-chars
JWT_REFRESH_SECRET=your-refresh-secret-key-min-32-chars
JWT_EXPIRES_IN=15m
JWT_REFRESH_EXPIRES_IN=7d

# External Services
STRIPE_SECRET_KEY=sk_test_xxxxx
SENDGRID_API_KEY=SG.xxxxx

# Optional
PORT=3000
LOG_LEVEL=debug
NODE_ENV=development
```
```

---

## 4. Dependency Installation

### 4.1 Package Installation Steps

**Node.js (npm/pnpm/yarn):**
```markdown
## Installation

### Clone the Repository

```bash
git clone https://github.com/{{OWNER}}/{{REPO}}.git
cd {{REPO}}
```

### Install Dependencies

```bash
# Using pnpm (recommended)
pnpm install

# Using npm
npm install

# Using yarn
yarn install
```

### Post-Install Scripts

The following scripts run automatically after installation:

| Script | Purpose |
|--------|---------|
| `postinstall` | Generates Prisma client |
| `prepare` | Sets up Git hooks via Husky |
```

**Python:**
```markdown
## Installation

### Clone the Repository

```bash
git clone https://github.com/{{OWNER}}/{{REPO}}.git
cd {{REPO}}
```

### Create Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate (macOS/Linux)
source venv/bin/activate

# Activate (Windows)
.\venv\Scripts\activate
```

### Install Dependencies

```bash
# Using pip
pip install -r requirements.txt

# For development
pip install -r requirements-dev.txt

# Using poetry
poetry install

# Using pipenv
pipenv install
```
```

### 4.2 Monorepo Installation

```markdown
## Installation (Monorepo)

This project uses a monorepo structure with multiple packages.

### Install All Dependencies

```bash
# From the root directory
pnpm install
```

### Build All Packages

```bash
# Build all packages in dependency order
pnpm build
```

### Package-Specific Installation

```bash
# Install dependencies for a specific package
pnpm --filter @project/api install

# Build a specific package
pnpm --filter @project/web build
```

### Workspace Structure

| Package | Path | Description |
|---------|------|-------------|
| `@project/api` | `apps/api` | Backend API server |
| `@project/web` | `apps/web` | Frontend application |
| `@project/shared` | `packages/shared` | Shared utilities |
| `@project/ui` | `packages/ui` | UI component library |
```

---

## 5. Database Setup

### 5.1 Database Setup Options

**Detect database tools:**
```
Glob: prisma/schema.prisma, alembic.ini, db/migrations, migrations/*.sql
Grep: typeorm|prisma|sequelize|mongoose|alembic|knex
```

### 5.2 Database Setup Documentation

```markdown
## Database Setup

### Option 1: Using Docker (Recommended)

```bash
# Start database containers
docker compose up -d postgres redis

# Verify containers are running
docker compose ps
```

### Option 2: Local Installation

#### PostgreSQL

**macOS:**
```bash
brew install postgresql@15
brew services start postgresql@15
createdb myapp_dev
```

**Ubuntu:**
```bash
sudo apt install postgresql-15
sudo -u postgres createuser --interactive
sudo -u postgres createdb myapp_dev
```

#### Redis

**macOS:**
```bash
brew install redis
brew services start redis
```

**Ubuntu:**
```bash
sudo apt install redis-server
sudo systemctl start redis
```

### Run Migrations

```bash
# Using Prisma
pnpm prisma migrate dev

# Using TypeORM
pnpm typeorm migration:run

# Using Alembic (Python)
alembic upgrade head

# Using Django
python manage.py migrate
```

### Seed Database (Optional)

```bash
# Run seed script
pnpm db:seed

# Or with Prisma
pnpm prisma db seed
```

### Database Management

| Command | Description |
|---------|-------------|
| `pnpm db:migrate` | Run pending migrations |
| `pnpm db:reset` | Drop and recreate database |
| `pnpm db:seed` | Seed with sample data |
| `pnpm db:studio` | Open Prisma Studio |
```

---

## 6. Third-Party Service Setup

### 6.1 Service Configuration Guide

```markdown
## Third-Party Services

### Stripe (Payment Processing)

1. Create a Stripe account at [stripe.com](https://stripe.com)
2. Navigate to [API Keys](https://dashboard.stripe.com/apikeys)
3. Copy the **Secret key** (starts with `sk_test_` for test mode)
4. Add to `.env`:
   ```
   STRIPE_SECRET_KEY=sk_test_xxxxx
   STRIPE_WEBHOOK_SECRET=whsec_xxxxx
   ```

**Webhook Setup (for development):**
```bash
# Install Stripe CLI
brew install stripe/stripe-cli/stripe

# Login
stripe login

# Forward webhooks to local server
stripe listen --forward-to localhost:3000/webhooks/stripe
```

### SendGrid (Email)

1. Create account at [sendgrid.com](https://sendgrid.com)
2. Go to Settings > API Keys
3. Create an API key with "Mail Send" permissions
4. Add to `.env`:
   ```
   SENDGRID_API_KEY=SG.xxxxx
   SENDGRID_FROM_EMAIL=noreply@yourdomain.com
   ```

### AWS S3 (File Storage)

1. Create an AWS account
2. Create an S3 bucket
3. Create an IAM user with S3 access
4. Add to `.env`:
   ```
   AWS_ACCESS_KEY_ID=AKIA...
   AWS_SECRET_ACCESS_KEY=xxxxx
   AWS_REGION=us-east-1
   AWS_S3_BUCKET=your-bucket-name
   ```

**Local Alternative (MinIO):**
```bash
# Run MinIO locally
docker compose up -d minio

# Access MinIO console at http://localhost:9001
# Default credentials: minioadmin/minioadmin
```
```

---

## 7. Local Development Setup

### 7.1 Development Server

```markdown
## Running Locally

### Start Development Server

```bash
# Start all services (database, cache, etc.)
docker compose up -d

# Start the development server
pnpm dev
```

The application will be available at:
- **API:** http://localhost:3000
- **Web:** http://localhost:3001
- **API Docs:** http://localhost:3000/api/docs

### Available Scripts

| Script | Description |
|--------|-------------|
| `pnpm dev` | Start development server with hot reload |
| `pnpm build` | Build for production |
| `pnpm start` | Start production server |
| `pnpm test` | Run test suite |
| `pnpm test:watch` | Run tests in watch mode |
| `pnpm lint` | Run ESLint |
| `pnpm lint:fix` | Fix linting issues |
| `pnpm type-check` | Run TypeScript type checking |

### Development Workflow

1. Start services: `docker compose up -d`
2. Start dev server: `pnpm dev`
3. Make changes (hot reload enabled)
4. Run tests: `pnpm test`
5. Commit changes: `git commit`
```

### 7.2 Watch Mode and Hot Reload

```markdown
### Hot Reload Configuration

The development server supports hot module replacement (HMR):

- **Backend:** Automatically restarts on file changes
- **Frontend:** Updates without page refresh
- **Styles:** Instant CSS updates

### Debugging

**VS Code Configuration:**

Add to `.vscode/launch.json`:
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug API",
      "type": "node",
      "request": "attach",
      "port": 9229,
      "restart": true
    }
  ]
}
```

Start with debugging:
```bash
pnpm dev:debug
```
```

---

## 8. Docker Setup

### 8.1 Docker Development Environment

```markdown
## Docker Setup

### Development with Docker Compose

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

### Docker Compose Services

| Service | Port | Description |
|---------|------|-------------|
| `api` | 3000 | API server |
| `web` | 3001 | Frontend server |
| `postgres` | 5432 | PostgreSQL database |
| `redis` | 6379 | Redis cache |
| `mailhog` | 8025 | Email testing UI |

### Building Docker Images

```bash
# Build all images
docker compose build

# Build specific service
docker compose build api

# Build with no cache
docker compose build --no-cache
```

### Docker Commands Reference

| Command | Description |
|---------|-------------|
| `docker compose up -d` | Start all services in background |
| `docker compose down` | Stop and remove containers |
| `docker compose down -v` | Also remove volumes |
| `docker compose logs api` | View API logs |
| `docker compose exec api sh` | Shell into API container |
| `docker compose restart api` | Restart API service |
```

### 8.2 Docker Production Build

```markdown
### Production Docker Build

```bash
# Build production image
docker build -t myapp:latest .

# Run production container
docker run -p 3000:3000 \
  -e NODE_ENV=production \
  -e DATABASE_URL=postgresql://... \
  myapp:latest
```

### Multi-Stage Build

The Dockerfile uses multi-stage builds for optimal image size:

| Stage | Purpose | Base Image |
|-------|---------|------------|
| `builder` | Install dependencies and build | node:18-alpine |
| `production` | Run application | node:18-alpine |

Final image size: ~150MB
```

---

## 9. CI/CD Setup

### 9.1 GitHub Actions Configuration

```markdown
## CI/CD Setup

### GitHub Actions

The project includes GitHub Actions workflows for:

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `ci.yml` | Push/PR | Run tests, lint, type-check |
| `deploy.yml` | Push to main | Deploy to production |
| `preview.yml` | PR | Deploy preview environment |

### Required Secrets

Add these secrets in GitHub repository settings:

| Secret | Description |
|--------|-------------|
| `DATABASE_URL` | Production database URL |
| `JWT_SECRET` | Production JWT secret |
| `STRIPE_SECRET_KEY` | Production Stripe key |
| `AWS_ACCESS_KEY_ID` | AWS credentials |
| `AWS_SECRET_ACCESS_KEY` | AWS credentials |
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub password |

### Local CI Testing

```bash
# Run the same checks as CI
pnpm lint
pnpm type-check
pnpm test
pnpm build
```
```

---

## 10. Troubleshooting

### 10.1 Common Issues

```markdown
## Troubleshooting

### Common Issues

#### Port Already in Use

```bash
# Find process using port 3000
lsof -i :3000

# Kill process
kill -9 <PID>

# Or use different port
PORT=3001 pnpm dev
```

#### Node Version Mismatch

```bash
# Check current version
node --version

# Install correct version with nvm
nvm install 18
nvm use 18

# Set as default
nvm alias default 18
```

#### Database Connection Failed

1. Verify database is running:
   ```bash
   docker compose ps
   docker compose logs postgres
   ```

2. Check connection string in `.env`

3. Test connection manually:
   ```bash
   psql $DATABASE_URL
   ```

#### Prisma Client Not Generated

```bash
# Regenerate Prisma client
pnpm prisma generate

# If schema changed, also run migration
pnpm prisma migrate dev
```

#### Permission Denied (Docker)

```bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER

# Restart session
newgrp docker
```

#### pnpm Not Found

```bash
# Install pnpm globally
npm install -g pnpm

# Or use corepack
corepack enable
corepack prepare pnpm@latest --activate
```

### Getting Help

If you encounter issues not covered here:

1. Check the [GitHub Issues](https://github.com/{{OWNER}}/{{REPO}}/issues)
2. Search existing issues for similar problems
3. Create a new issue with:
   - Node.js version (`node --version`)
   - Operating system
   - Error message (full stack trace)
   - Steps to reproduce
```

---

## 11. Output Format

Generate setup guide following this structure:

```markdown
# {{PROJECT_NAME}} Setup Guide

## Prerequisites

{{PREREQUISITES_LIST}}

## Quick Start

{{QUICK_START_STEPS}}

## Detailed Setup

### 1. Environment Configuration

{{ENVIRONMENT_SETUP}}

### 2. Dependency Installation

{{INSTALLATION_STEPS}}

### 3. Database Setup

{{DATABASE_SETUP}}

### 4. Third-Party Services

{{SERVICE_CONFIGURATION}}

## Running Locally

{{LOCAL_DEV_INSTRUCTIONS}}

## Docker Setup

{{DOCKER_INSTRUCTIONS}}

## CI/CD

{{CICD_SETUP}}

## Troubleshooting

{{TROUBLESHOOTING}}

## Next Steps

{{NEXT_STEPS}}
```

---

## 12. Quality Checks

Before finalizing, verify:

- [ ] All commands are copy-paste ready
- [ ] Prerequisites are complete and accurate
- [ ] Environment variables are all documented
- [ ] Database setup covers both Docker and local
- [ ] Commands work on macOS, Windows, and Linux
- [ ] Troubleshooting covers common issues
- [ ] Links to external resources are valid
- [ ] Scripts reference matches package.json
- [ ] No sensitive data in examples
