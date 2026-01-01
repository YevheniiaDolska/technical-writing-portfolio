# Technical Writing Portfolio

[![Documentation](https://img.shields.io/badge/Documentation-Live-brightgreen)](https://yevheniiadolska.github.io/technical-writing-portfolio/)
[![MkDocs](https://img.shields.io/badge/Built%20with-MkDocs-blue)](https://www.mkdocs.org/)

Professional technical writing portfolio demonstrating API documentation, developer guides, and technical communication skills.

## Live Portfolio

**[View Live Documentation →](https://yevheniiadolska.github.io/technical-writing-portfolio/)**

**[View Source on GitHub →](https://github.com/YevheniiaDolska/technical-writing-portfolio)**

## Portfolio Contents

This portfolio showcases various types of technical documentation:

### API Documentation
- **[API Overview](api-documentation/overview.md)** - RESTful API introduction with authentication, request/response structures, and best practices
- **[API Endpoints Reference](api-documentation/api-endpoints.md)** - Detailed endpoint documentation with parameters, examples, and error handling

### Developer Portal
- **[Developer Portal Overview](developer-portal/index.md)** - Interactive API documentation samples
- **[OpenAPI Specification](developer-portal/openapi/index.md)** - Complete OpenAPI 3.0 spec with Swagger UI sandbox
- **[SDK Documentation](developer-portal/sdk/httpx-guide.md)** - Comprehensive Python library documentation

### Developer Guides
- **[Quick Start Guide](tutorials/quick-start-guide.md)** - Step-by-step tutorial for first-time API users
- **[Architecture Overview](developer-guides/architecture.md)** - System design documentation with diagrams and component descriptions

### Release Documentation
- **[Release Notes](release-notes/release-notes.md)** - Version updates, migration guides, and breaking changes

### Confluence Samples
- **[Confluence Documentation Samples](confluence-samples.md)** - User guides, feature descriptions, and admin documentation created in Confluence

## Technical Stack

This documentation site is built using modern docs-as-code practices:

- **Static Site Generator:** MkDocs with Material theme
- **Version Control:** Git & GitHub
- **CI/CD:** GitHub Actions for automated deployment
- **Hosting:** GitHub Pages
- **Diagrams:** Mermaid for architecture diagrams
- **Search:** Built-in search functionality

## Key Features

- **Responsive Design** - Works on desktop, tablet, and mobile
- **Dark Mode** - Automatic theme switching
- **Full-Text Search** - Instantly searchable documentation
- **Code Highlighting** - Syntax highlighting for multiple languages
- **Navigation** - Clear hierarchy and breadcrumbs
- **Print-Friendly** - Optimized CSS for printing

## Project Structure

```
technical-writing-portfolio/
│
├── docs/                      # Documentation source files
│   ├── index.md              # Homepage
│   ├── CV.md                 # About the technical writer
│   ├── confluence-samples.md # Confluence samples overview
│   ├── api-documentation/    # API docs
│   │   ├── overview.md
│   │   └── api-endpoints.md
│   ├── developer-portal/     # Developer Portal samples
│   │   ├── index.md         # Portal overview
│   │   ├── openapi/         # OpenAPI specifications
│   │   │   ├── index.md     # Interactive Swagger UI
│   │   │   └── pokeapi-spec.yaml
│   │   └── sdk/             # SDK documentation
│   │       └── httpx-guide.md
│   ├── tutorials/            # Guides and tutorials
│   │   └── quick-start-guide.md
│   ├── developer-guides/     # Technical guides
│   │   └── architecture.md
│   ├── release-notes/        # Version documentation
│   │   └── release-notes.md
│   ├── confluence-samples/   # PDF samples
│   │   ├── user-guide-sample_1.pdf
│   │   ├── feature_description.pdf
│   │   └── admin_documentation.pdf
│   └── images/               # Images
│       └── profile.jpg
├── .github/workflows/        # CI/CD
│   └── deploy.yml           # GitHub Pages deployment
├── mkdocs.yml               # MkDocs configuration
└── requirements.txt         # Python dependencies
```

## Documentation Standards

This portfolio follows industry best practices:

- **Style Guide:** Microsoft Writing Style Guide
- **API Documentation:** OpenAPI Specification compatible
- **Markdown:** CommonMark specification
- **Version Control:** Semantic versioning
- **Accessibility:** WCAG 2.1 AA compliant

## Skills Demonstrated

### Technical Writing
- API reference documentation
- OpenAPI/Swagger specifications
- SDK and library documentation
- Developer tutorials and guides
- Architecture documentation
- Release notes and changelogs
- Quick start guides
- Interactive documentation with Swagger UI

### Technical Skills
- Markdown and reStructuredText
- OpenAPI 3.0 specification
- Git version control
- Static site generators
- CI/CD pipelines
- REST API understanding
- JSON/YAML formatting
- Basic HTML/CSS/JavaScript

### Tools & Platforms
- MkDocs with Material theme
- Swagger UI / Swagger Editor
- Redoc
- Git, GitHub, GitHub Pages, GitHub Actions
- ReadMe.com
- Stoplight
- Redocly
- Postman
- Draw.io, Mermaid, PlantUML
- VS Code
- Confluence
- Jira
- GitBook
- RoboHelp
