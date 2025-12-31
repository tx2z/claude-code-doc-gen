# Claude Code Documentation Generator

A comprehensive documentation generation command for [Claude Code](https://claude.ai/claude-code) that creates professional-quality documentation using specialized AI agents.

## Features

- **README Generation** - Complete project README with badges, installation, usage, and more
- **API Documentation** - REST/GraphQL endpoint docs, OpenAPI specs, Postman collections
- **Architecture Docs** - System diagrams, data flows, ADRs using Mermaid
- **Setup Guides** - Prerequisites, installation, configuration, troubleshooting
- **Inline Comments** - JSDoc, docstrings, type annotations, complexity explanations
- **Component Docs** - React/Vue props documentation, Storybook stories

## Requirements

- [Claude Code CLI](https://claude.ai/claude-code) installed and configured
- A project to document

## Installation

1. Clone or download this repository
2. Copy the folders to your project's `.claude/` directory:

```bash
# From your project root
cp -r path/to/claude-code-doc-gen/commands .claude/
cp -r path/to/claude-code-doc-gen/docs .claude/
```

Your project structure should look like:
```
your-project/
├── .claude/
│   ├── commands/
│   │   └── doc-gen.md
│   └── docs/
│       ├── agents/
│       │   ├── readme-generator.md
│       │   ├── api-documentation.md
│       │   ├── architecture-docs.md
│       │   ├── setup-guide.md
│       │   ├── inline-comments.md
│       │   └── component-docs.md
│       └── templates/
│           ├── README.template.md
│           ├── API.template.md
│           ├── ARCHITECTURE.template.md
│           ├── SETUP.template.md
│           ├── CONTRIBUTING.template.md
│           └── CHANGELOG.template.md
├── src/
└── ...
```

3. (Optional) Add `docs-output/` to your `.gitignore` if you don't want generated docs committed:

```bash
echo "docs-output/" >> .gitignore
```

## Usage

In Claude Code, run the documentation generator command:

```
/doc-gen
```

### Generation Modes

| Command | Description |
|---------|-------------|
| `/doc-gen` | Interactive mode - choose what to generate |
| `/doc-gen full` | Generate all documentation types |
| `/doc-gen readme` | Generate README.md only |
| `/doc-gen api` | Generate API documentation |
| `/doc-gen arch` | Generate architecture documentation |
| `/doc-gen setup` | Generate setup/installation guide |
| `/doc-gen contrib` | Generate CONTRIBUTING.md |
| `/doc-gen changelog` | Generate or update CHANGELOG.md |
| `/doc-gen inline` | Add/improve inline code comments |
| `/doc-gen storybook` | Generate Storybook stories for components |

### Output Formats

| Format | Command Flag | Description |
|--------|--------------|-------------|
| Markdown | (default) | Standard .md files |
| HTML | `--format html` | Standalone HTML pages |
| MDX | `--format mdx` | MDX for Docusaurus/Next.js |
| Notion | `--format notion` | Notion-compatible markdown |
| Confluence | `--format confluence` | Confluence wiki format |

Example: `/doc-gen api --format html`

## Documentation Agents

### DOC01 - README Generator

Generates comprehensive README.md including:
- Project title and badges (build, coverage, npm, license)
- Description and features list
- Tech stack overview
- Prerequisites and installation
- Quick start guide
- Configuration options
- Usage examples with code snippets
- Available scripts reference
- Project structure
- Contributing guidelines
- License information

### DOC02 - API Documentation

For REST APIs:
- Endpoint listing with methods and paths
- Request/response schemas
- Authentication requirements
- Rate limiting information
- Error codes and handling
- Example requests (curl, fetch, axios)
- OpenAPI/Swagger spec generation
- Postman collection export

For Libraries:
- Function signatures
- Parameter descriptions
- Return types
- Usage examples
- Edge cases

### DOC03 - Architecture Documentation

Generates:
- System overview diagram (Mermaid)
- Component diagram
- Data flow diagram
- Sequence diagrams for key flows
- Entity relationship diagram
- Deployment architecture
- Technology decisions (ADRs)
- Integration points
- Security architecture overview

### DOC04 - Setup Guide

Creates:
- Prerequisites checklist
- Environment setup instructions
- Dependency installation steps
- Configuration walkthrough
- Database setup
- Third-party service setup
- Local development guide
- Docker setup instructions
- CI/CD configuration
- Troubleshooting common issues

### DOC05 - Inline Comments

Adds/improves:
- Function documentation (JSDoc, Python docstrings)
- Complex logic explanations
- TODO/FIXME standardization
- Type annotations
- Example usage in comments
- Performance notes
- Security considerations

### DOC06 - Component Documentation

For React/Vue components:
- Props documentation
- Usage examples
- Variants showcase
- Accessibility notes
- Storybook stories generation
- Test scenarios

## Supported Tech Stacks

The generator auto-detects and adapts to:

**Languages:**
- TypeScript/JavaScript
- Python
- PHP
- .NET/C#
- Go
- Java
- Rust

**Frontend Frameworks:**
- React, Next.js
- Vue, Nuxt
- Angular
- Svelte

**Backend Frameworks:**
- Express, NestJS, Fastify
- Django, Flask, FastAPI
- Laravel, Symfony
- ASP.NET Core
- Gin, Echo, Fiber
- Spring Boot

**Documentation Tools:**
- Storybook
- Swagger/OpenAPI
- TypeDoc, JSDoc
- Sphinx, MkDocs
- VuePress, Docusaurus

## How It Works

1. **Project Analysis** - Scans your codebase to understand structure, dependencies, and patterns
2. **Tech Stack Detection** - Identifies frameworks, languages, and tools in use
3. **Agent Execution** - Spawns specialized documentation agents based on mode
4. **Content Generation** - Creates documentation following best practices
5. **Template Application** - Applies consistent formatting using templates
6. **Output Writing** - Writes documentation to appropriate locations

## Output Locations

| Documentation Type | Default Location |
|-------------------|------------------|
| README | `./README.md` |
| API Docs | `./docs/api/` |
| Architecture | `./docs/architecture/` |
| Setup Guide | `./docs/SETUP.md` |
| Contributing | `./CONTRIBUTING.md` |
| Changelog | `./CHANGELOG.md` |
| Storybook | `./src/stories/` |

## Customization

To adapt for your specific needs:

1. **Templates** - Modify templates in `docs/templates/` to match your style
2. **Agent Behavior** - Edit individual agents in `docs/agents/`
3. **Output Paths** - Change paths in `doc-gen.md` command
4. **Badges** - Customize badge generation in README template

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file.

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Mermaid Diagrams](https://mermaid.js.org/)
- [OpenAPI Specification](https://swagger.io/specification/)
- [JSDoc](https://jsdoc.app/)
- [Storybook](https://storybook.js.org/)
