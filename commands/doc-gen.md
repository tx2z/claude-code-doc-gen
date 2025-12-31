---
description: Generate comprehensive documentation for your project using specialized agents
allowed-tools: Bash(ls:*), Bash(cat:*), Bash(find:*), Read, Glob, Grep, Edit, Write, Task, AskUserQuestion, WebSearch
argument-hint: "[mode: full|readme|api|arch|setup|contrib|changelog|inline|storybook] [--format markdown|html|mdx|notion|confluence]"
---

# Documentation Generator

Comprehensive documentation generation using specialized AI agents. Supports multiple output formats and adapts to your project's tech stack.

## Generation Modes

Parse `$ARGUMENTS` to determine mode and format:

| Argument | Description |
|----------|-------------|
| (none) | Interactive mode - ask user what to generate |
| `full` | Generate all documentation types |
| `readme` | Generate README.md only |
| `api` | Generate API documentation |
| `arch` | Generate architecture documentation |
| `setup` | Generate setup/installation guide |
| `contrib` | Generate CONTRIBUTING.md |
| `changelog` | Generate or update CHANGELOG.md |
| `inline` | Add/improve inline code comments |
| `storybook` | Generate Storybook stories for components |

### Format Flags

| Flag | Output Format |
|------|---------------|
| `--format markdown` | Standard Markdown (default) |
| `--format html` | Standalone HTML pages |
| `--format mdx` | MDX for Docusaurus/Next.js |
| `--format notion` | Notion-compatible markdown |
| `--format confluence` | Confluence wiki format |

---

## Step 1: Project Analysis

Analyze the project to understand its structure and purpose.

### 1.1 Identify Project Type

**Check for indicators:**

```
Glob: package.json, requirements.txt, composer.json, go.mod, Cargo.toml, *.csproj, pom.xml, build.gradle
```

**Determine project type:**
- Library/Package (publishable code)
- Web Application (frontend/backend)
- CLI Tool
- API Service
- Monorepo
- Mobile App

### 1.2 Detect Tech Stack

**JavaScript/TypeScript:**
```
Glob: package.json, tsconfig.json, *.ts, *.tsx, *.js, *.jsx
Grep: react|vue|angular|svelte|next|nuxt|express|nestjs|fastify
```

**Python:**
```
Glob: requirements.txt, setup.py, pyproject.toml, *.py
Grep: django|flask|fastapi|pytest|numpy|pandas
```

**PHP:**
```
Glob: composer.json, *.php
Grep: laravel|symfony|wordpress
```

**.NET/C#:**
```
Glob: *.csproj, *.sln, *.cs
Grep: ASP\.NET|Entity Framework|Blazor
```

**Go:**
```
Glob: go.mod, go.sum, *.go
Grep: gin|echo|fiber|chi
```

**Java:**
```
Glob: pom.xml, build.gradle, *.java
Grep: spring|hibernate|jakarta
```

**Rust:**
```
Glob: Cargo.toml, *.rs
Grep: actix|rocket|tokio|serde
```

### 1.3 Scan Project Structure

**Common directories:**
```
ls -la
ls src/ || ls app/ || ls lib/
ls tests/ || ls test/ || ls __tests__/
ls docs/ || ls documentation/
ls .github/ || ls .gitlab/
```

**Identify key files:**
- Entry points (main, index, app)
- Configuration files
- Test files
- Existing documentation
- CI/CD configuration

### 1.4 Extract Project Metadata

**From package.json (Node.js):**
- name, version, description
- scripts (build, test, start, lint)
- dependencies, devDependencies
- repository, author, license

**From pyproject.toml/setup.py (Python):**
- name, version, description
- dependencies, dev-dependencies
- scripts/entry points

**From other manifests:**
- Similar metadata extraction

### 1.5 Present Analysis to User

```
=== Project Analysis ===

Project: {{PROJECT_NAME}}
Type: {{PROJECT_TYPE}}
Version: {{VERSION}}

Tech Stack:
- Language: {{LANGUAGE}}
- Framework: {{FRAMEWORK}}
- Testing: {{TEST_FRAMEWORK}}
- Build: {{BUILD_TOOL}}

Structure:
- Source: {{SOURCE_DIR}}
- Tests: {{TEST_DIR}}
- Docs: {{DOCS_DIR}}

Detected Features:
- [ ] API endpoints
- [ ] React/Vue components
- [ ] Database models
- [ ] CLI commands
- [ ] Configuration options

Is this analysis correct? Any additions or corrections?
```

Use `AskUserQuestion` to confirm or get corrections.

---

## Step 2: Mode Selection

If no mode specified in `$ARGUMENTS`, present options:

```
=== Documentation Generator ===

What would you like to generate?

1. Full Documentation (all types)
2. README.md
3. API Documentation
4. Architecture Docs
5. Setup Guide
6. CONTRIBUTING.md
7. CHANGELOG.md
8. Inline Comments
9. Storybook Stories

Enter your choice (1-9) or comma-separated list (e.g., 2,3,5):
```

---

## Step 3: Execute Documentation Agents

Based on mode, spawn Task agents for each documentation type.

### Full Mode Agents (run sequentially)

**Agent 1: README Generator (DOC01)**
- Read: `.claude/docs/agents/readme-generator.md`
- Template: `.claude/docs/templates/README.template.md`
- Output: `./README.md`

**Agent 2: API Documentation (DOC02)**
- Condition: Only if API endpoints detected
- Read: `.claude/docs/agents/api-documentation.md`
- Template: `.claude/docs/templates/API.template.md`
- Output: `./docs/api/`

**Agent 3: Architecture Documentation (DOC03)**
- Read: `.claude/docs/agents/architecture-docs.md`
- Template: `.claude/docs/templates/ARCHITECTURE.template.md`
- Output: `./docs/architecture/`

**Agent 4: Setup Guide (DOC04)**
- Read: `.claude/docs/agents/setup-guide.md`
- Template: `.claude/docs/templates/SETUP.template.md`
- Output: `./docs/SETUP.md` or `./SETUP.md`

**Agent 5: Contributing Guide**
- Template: `.claude/docs/templates/CONTRIBUTING.template.md`
- Output: `./CONTRIBUTING.md`

**Agent 6: Changelog**
- Template: `.claude/docs/templates/CHANGELOG.template.md`
- Output: `./CHANGELOG.md`

### Conditional Agents

**Agent 7: Component Documentation (DOC06)**
- Condition: Only if React/Vue/Angular components detected
- Read: `.claude/docs/agents/component-docs.md`
- Output: Component-level docs and Storybook stories

**Agent 8: Inline Comments (DOC05)**
- Read: `.claude/docs/agents/inline-comments.md`
- Modifies source files directly

---

## Step 4: Output Format Handling

Based on `--format` flag, transform output:

### Markdown (default)
- Standard GitHub-flavored markdown
- Mermaid diagrams inline
- Relative links

### HTML
- Convert markdown to standalone HTML
- Embed CSS styling
- Include Mermaid.js for diagrams

### MDX
- Add frontmatter for Docusaurus/Next.js
- Import React components
- Export metadata

### Notion
- Adjust heading levels
- Convert Mermaid to code blocks
- Use Notion-compatible callouts

### Confluence
- Convert to Confluence wiki markup
- Use {code} blocks for snippets
- Use Confluence macros

---

## Step 5: Write Output

For each generated document:

1. **Check for existing files**
   - If exists, ask user: Overwrite, Merge, or Skip?
   - For merge, show diff preview

2. **Create directories if needed**
   ```
   mkdir -p docs/api docs/architecture
   ```

3. **Write files**
   - Use appropriate encoding (UTF-8)
   - Add generated timestamp comment

4. **Validate output**
   - Check markdown syntax
   - Verify links work
   - Ensure diagrams render

---

## Step 6: Summary Report

After generation:

```
=== Documentation Generation Complete ===

Generated Files:
- [x] README.md (2,450 words)
- [x] docs/api/endpoints.md (1,200 words)
- [x] docs/architecture/overview.md (800 words)
- [x] CONTRIBUTING.md (450 words)
- [x] CHANGELOG.md (300 words)

Skipped:
- [ ] Storybook stories (no components found)
- [ ] Inline comments (user declined)

Total: 5 files, 5,200 words

Next steps:
1. Review generated documentation
2. Add any missing context or details
3. Commit changes: git add docs/ README.md CONTRIBUTING.md CHANGELOG.md

Would you like to review any specific file?
```

---

## Agent Reference

| ID | Agent | Purpose | Trigger |
|----|-------|---------|---------|
| DOC01 | readme-generator | README.md generation | `readme`, `full` |
| DOC02 | api-documentation | API docs, OpenAPI specs | `api`, `full` (if APIs) |
| DOC03 | architecture-docs | System diagrams, ADRs | `arch`, `full` |
| DOC04 | setup-guide | Installation guide | `setup`, `full` |
| DOC05 | inline-comments | Code comments | `inline` |
| DOC06 | component-docs | React/Vue component docs | `storybook`, `full` (if components) |

---

## Tech Stack Patterns

### TypeScript/JavaScript
- JSDoc comments
- TypeScript types for documentation
- README with npm badges
- package.json scripts section

### Python
- Sphinx/MkDocs style
- Google/NumPy docstrings
- setup.py/pyproject.toml metadata
- pytest examples

### PHP
- PHPDoc comments
- Composer metadata
- PSR standards references

### .NET/C#
- XML documentation comments
- NuGet package metadata
- MSBuild targets

### Go
- GoDoc style comments
- go.mod dependencies
- Effective Go references

### Java
- Javadoc comments
- Maven/Gradle dependencies
- Spring annotations

### Rust
- rustdoc comments
- Cargo.toml metadata
- Examples directory

---

## Customization Points

To adapt for other projects:

1. **Templates** - Modify files in `docs/templates/`
2. **Agent prompts** - Edit files in `docs/agents/`
3. **Output paths** - Change paths in Step 5
4. **Format converters** - Extend Step 4 for new formats
5. **Tech detection** - Add patterns in Step 1.2
