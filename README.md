# Arc42 Architecture Documentation - B2B E-Commerce Data Pipeline

Production-ready multilingual (DE/EN) arc42 architecture documentation repository with strict symmetry governance, automated validation, and CI/CD pipeline.

## Quick Start

### Prerequisites
- Java 17 or higher
- Git

### Clone and Build

```bash
git clone <repository-url>
cd arc42-architecture-documentation

# Build HTML and PDF documentation for both languages
./gradlew buildAll

# Build only German documentation
./gradlew asciidoctorHtmlDE asciidoctorPdfDE

# Build only English documentation
./gradlew asciidoctorHtmlEN asciidoctorPdfEN
```

### Output Locations

After building, documentation is available in:
- **German HTML**: `build/DE/html/arc42-template.html`
- **German PDF**: `build/DE/pdf/arc42-template.pdf`
- **English HTML**: `build/EN/html/arc42-template.html`
- **English PDF**: `build/EN/pdf/arc42-template.pdf`

## Repository Structure

```
.
├── DE/                          # German documentation
│   └── asciidoc/
│       ├── arc42-template.adoc  # Main document
│       ├── src/                 # Chapter files
│       └── version.properties   # Version metadata
│
├── EN/                          # English documentation
│   └── asciidoc/
│       ├── arc42-template.adoc  # Main document
│       ├── src/                 # Chapter files
│       └── version.properties   # Version metadata
│
├── images/
│   ├── shared/                  # Language-independent diagrams
│   ├── DE/                      # German screenshots
│   └── EN/                      # English screenshots
│
├── .github/
│   ├── workflows/               # CI/CD pipelines
│   └── PULL_REQUEST_TEMPLATE.md
│
├── build.gradle                 # Gradle build configuration
├── gradle.properties            # Build properties
└── settings.gradle
```

## Symmetry Rule

**CRITICAL**: DE and EN directories must maintain **strict structural symmetry**.

This means:
- Same number of `.adoc` files in `DE/asciidoc/src/` and `EN/asciidoc/src/`
- Identical filenames in both language directories
- Identical folder structure
- Only content (text) differs between languages

### Enforced By:
1. **Pre-commit Git Hook**: Blocks asymmetric commits locally
2. **GitHub Actions**: Validates symmetry in CI/CD pipeline
3. **Pull Request Template**: Checklist for reviewers

## Editing Documentation

### Adding New Chapter

1. Create file in **both** DE and EN:
   ```bash
   touch DE/asciidoc/src/13_new_chapter.adoc
   touch EN/asciidoc/src/13_new_chapter.adoc
   ```

2. Add content to both files (same structure, different language)

3. Include in main templates:
   ```asciidoc
   // In DE/asciidoc/arc42-template.adoc and EN/asciidoc/arc42-template.adoc
   include::src/13_new_chapter.adoc[]
   ```

4. Test build:
   ```bash
   ./gradlew buildAll
   ```

### Adding Images

**Language-specific screenshots** (UI text visible):
```asciidoc
// In DE document
image::../../images/DE/my-screenshot-de.png[Description]

// In EN document
image::../../images/EN/my-screenshot-en.png[Description]
```

**Language-independent diagrams** (no text or technical only):
```asciidoc
// In both DE and EN documents
image::../../images/shared/architecture-diagram.png[Description]
```

### Working with Subfolders

When creating subchapter structures:

```bash
# Create folder in BOTH languages
mkdir -p DE/asciidoc/src/05_building_blocks/05_4_integrations
mkdir -p EN/asciidoc/src/05_building_blocks/05_4_integrations

# Add files to BOTH
touch DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc
touch EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc
```

## Development Workflow

### 1. Before Starting

```bash
# Ensure you're on the right branch
git checkout -b feature/add-payment-gateway

# Verify current structure
./gradlew buildAll
```

### 2. Make Changes

Edit files in **both** `DE/` and `EN/` directories.

### 3. Test Locally

```bash
# Build to verify AsciiDoc syntax
./gradlew buildAll

# Check output
open build/DE/html/arc42-template.html
open build/EN/html/arc42-template.html
```

### 4. Commit

The pre-commit hook will automatically check symmetry:

```bash
git add .
git commit -m "docs: Add payment gateway documentation"

# If symmetry check fails, you'll see an error
# Fix the issue and try again
```

### 5. Push and Create PR

```bash
git push origin feature/add-payment-gateway
```

The GitHub Actions workflow will:
- Validate DE/EN symmetry
- Build documentation for both languages
- Upload artifacts

## CI/CD Pipeline

### Triggered On:
- Pull requests affecting `DE/asciidoc/**`, `EN/asciidoc/**`, or `images/**`
- Pushes to `main` branch

### Pipeline Steps:
1. **Symmetry Check**: Validates file count, filenames, and folder structure
2. **Build**: Generates HTML and PDF for both languages
3. **Artifact Upload**: Makes built documentation available for download

### View Pipeline Results:
Go to **Actions** tab in GitHub repository.

## VS Code Setup

### Recommended Extension:
- **AsciiDoc** by asciidoctor

### Features Enabled:
- Live preview
- Syntax highlighting
- Kroki diagram support
- Word wrap at 80/120 characters

Settings are pre-configured in `.vscode/settings.json`.

## Example Use Case

This repository documents a **B2B E-Commerce Data Pipeline** with:
- Order management system
- Payment processing (Stripe integration)
- ERP system integration
- Marketplace connections

### Key Documentation Areas:
- **Chapter 1**: Introduction and quality goals
- **Chapter 5.4**: External integrations (Stripe, ERP, Marketplace)
- **Chapter 6**: Runtime flows (Order → Payment → ERP sync)
- **Chapter 8**: Cross-cutting concepts (logging, error handling, auth)

## Troubleshooting

### Pre-commit Hook Fails

```
❌ ERROR: DE und EN haben unterschiedliche Dateizahl!
```

**Solution**: Ensure every `.adoc` file in `DE/` has a corresponding file in `EN/` with the same name.

### Build Fails with "File Not Found"

**Cause**: An `include::` statement references a file that doesn't exist.

**Solution**: Check all `include::` statements in `arc42-template.adoc` match actual files in `src/`.

### Images Not Showing in Output

**Solution**: Verify image paths are relative:
- Correct: `../../images/shared/diagram.png`
- Wrong: `/images/shared/diagram.png`

### Gradle Build Errors

```bash
# Clean build cache
./gradlew clean

# Rebuild
./gradlew buildAll
```

## Contributing

1. Read `ARCHITECTURE_DOCS_GUIDE.md` for detailed guidelines
2. Always maintain DE/EN symmetry
3. Use the PR template checklist
4. Verify builds pass locally before pushing

## Version

Current version: **1.0**
Build date: **2025-11-12**

## License

[Specify your license here]

## Support

For issues or questions, please open a GitHub issue.
