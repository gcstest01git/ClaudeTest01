# CLAUDE.md - AI Assistant Guide for arc42-masterdata

## Project Overview

**Repository:** arc42-masterdata
**Type:** Multilingual (DE/EN) Arc42 Architecture Documentation
**Project:** B2B E-Commerce Data Pipeline
**Documentation Format:** AsciiDoc
**Build System:** Gradle with Asciidoctor plugins
**Version:** 1.0 (Build: 2025-11-12)
**Java Requirement:** 17+

This repository contains production-ready arc42 architecture documentation with **strict DE/EN symmetry governance**, automated validation, and CI/CD pipeline. The documentation describes a B2B e-commerce data pipeline system with order management, payment processing (Stripe), ERP integration, and marketplace connections.

---

## 🚨 CRITICAL RULE: Symmetry Is Sacred

### The Golden Rule

**EVERY structural element in `DE/` MUST have an identical counterpart in `EN/`.**

This means:
- ✅ Same number of `.adoc` files in `DE/asciidoc/src/` and `EN/asciidoc/src/`
- ✅ Identical filenames (including case, underscores, extensions)
- ✅ Identical folder structure and hierarchy
- ✅ Only text content differs between languages

**This is enforced by:**
1. Pre-commit git hooks (blocks local commits)
2. GitHub Actions CI/CD (blocks PRs)
3. Pull request template checklist

### Why This Matters for AI Assistants

When you create, modify, or delete files:
- **ALWAYS** perform the same operation in BOTH `DE/` and `EN/` directories
- **NEVER** create a file in only one language
- **NEVER** delete a file from only one language
- **NEVER** rename a file in only one language

**Example - CORRECT:**
```bash
# User: "Add documentation for Redis caching"
1. Create DE/asciidoc/src/08_crosscutting/08_5_caching.adoc
2. Create EN/asciidoc/src/08_crosscutting/08_5_caching.adoc
3. Write German content to DE version
4. Write English content to EN version
```

**Example - INCORRECT (Will fail CI):**
```bash
# ❌ WRONG - Only creates German version
1. Create DE/asciidoc/src/08_crosscutting/08_5_caching.adoc
# Missing: EN/asciidoc/src/08_crosscutting/08_5_caching.adoc
```

---

## Repository Structure

```
arc42-masterdata/
├── DE/                              # German documentation
│   └── asciidoc/
│       ├── arc42-template.adoc      # Main German template (includes all chapters)
│       ├── version.properties       # Version metadata (v1.0, 2025-11-12)
│       └── src/                     # Chapter source files (24 files)
│           ├── 01_introduction.adoc
│           ├── 02_constraints.adoc
│           ├── 03_context.adoc
│           ├── 04_solution_strategy.adoc
│           ├── 05_building_blocks/       # Hierarchical structure
│           │   ├── 05_1_overview.adoc
│           │   ├── 05_2_frontend.adoc
│           │   ├── 05_3_backend.adoc
│           │   ├── 05_4_integrations.adoc
│           │   ├── 05_4_integrations/    # Nested integrations
│           │   │   ├── 05_4_1_stripe.adoc
│           │   │   ├── 05_4_2_marketplace.adoc
│           │   │   └── 05_4_3_erp.adoc
│           │   └── 05_5_data_layer.adoc
│           ├── 06_runtime/
│           │   ├── 06_1_order_flow.adoc
│           │   ├── 06_2_payment_flow.adoc
│           │   └── 06_3_integration_flow.adoc
│           ├── 07_deployment.adoc
│           ├── 08_crosscutting/
│           │   ├── 08_1_logging.adoc
│           │   ├── 08_2_error_handling.adoc
│           │   ├── 08_3_authentication.adoc
│           │   └── 08_4_api_conventions.adoc
│           ├── 09_decisions.adoc
│           ├── 10_quality.adoc
│           ├── 11_risks.adoc
│           └── 12_glossary.adoc
│
├── EN/                              # English documentation (IDENTICAL structure to DE/)
│   └── asciidoc/                    # Same 24 files, English content
│       ├── arc42-template.adoc
│       ├── version.properties
│       └── src/                     # Mirror of DE/asciidoc/src/
│
├── images/
│   ├── shared/                      # Language-independent diagrams
│   ├── DE/                          # German screenshots (UI text in German)
│   └── EN/                          # English screenshots (UI text in English)
│
├── .github/
│   ├── workflows/
│   │   └── check-symmetry.yml       # CI/CD: Validates symmetry, builds docs
│   └── PULL_REQUEST_TEMPLATE.md     # Checklist for symmetry compliance
│
├── build.gradle                     # Gradle build configuration
├── gradle.properties                # languages=DE,EN, parallel=true
├── settings.gradle                  # rootProject.name
├── README.md                        # User-facing documentation
├── ARCHITECTURE_DOCS_GUIDE.md       # Comprehensive editing guide
└── CLAUDE.md                        # This file (AI assistant guide)
```

---

## File Naming Conventions

### AsciiDoc Files

**Pattern:** `{chapter_number}_{descriptive_name}.adoc`

**Rules:**
- Use lowercase only
- Use underscores (not hyphens or spaces)
- Include chapter numbers for ordering (01, 02, 05_4_1, etc.)
- Be descriptive but concise
- Use `.adoc` extension

**Examples:**
- ✅ `01_introduction.adoc`
- ✅ `05_4_1_stripe.adoc`
- ✅ `08_2_error_handling.adoc`
- ❌ `Introduction.adoc` (no chapter number, wrong case)
- ❌ `05-stripe.adoc` (hyphens instead of underscores)
- ❌ `08 error handling.adoc` (spaces in filename)

### Image Files

**Pattern:** `{component}-{purpose}-{language}.{ext}`

**Examples:**
- `stripe-checkout-de.png` (German screenshot)
- `stripe-checkout-en.png` (English screenshot)
- `architecture-overview.drawio.png` (shared, no language suffix)
- `admin-dashboard-de.png` / `admin-dashboard-en.png`

---

## Image Management Strategy

### Three Categories

#### 1. Shared Images (`images/shared/`)

**Use for:**
- Technical diagrams without text
- Architecture overviews with only technical terms
- Database schemas
- UML diagrams
- Flowcharts with minimal/technical text

**Reference in BOTH DE and EN documents:**
```asciidoc
image::../../images/shared/architecture-overview.drawio.png[Architecture,800,align=center]
```

#### 2. German Images (`images/DE/`)

**Use for:**
- Screenshots with German UI text
- Diagrams with German labels/annotations
- Mockups with German content

**Reference in DE documents:**
```asciidoc
image::../../images/DE/stripe-checkout-de.png[Stripe Checkout,600]
```

#### 3. English Images (`images/EN/`)

**Use for:**
- Screenshots with English UI text
- Diagrams with English labels/annotations
- Mockups with English content

**Reference in EN documents:**
```asciidoc
image::../../images/EN/stripe-checkout-en.png[Stripe Checkout,600]
```

### Important Path Rules

**ALWAYS use relative paths:**
- From `arc42-template.adoc`: `../../images/shared/diagram.png`
- Or set `imagesdir` attribute in template: `:imagesdir: ../../images`
- Then reference as: `image::shared/diagram.png[]`

**The `:imagesdir: ../../images` is already set in arc42-template.adoc (line 8)**

---

## Common Tasks for AI Assistants

### Task 1: Add a New Chapter Section

**User Request:** "Add documentation for Redis caching to chapter 8"

**Steps:**
1. Create files in BOTH languages:
   ```bash
   touch DE/asciidoc/src/08_crosscutting/08_5_caching.adoc
   touch EN/asciidoc/src/08_crosscutting/08_5_caching.adoc
   ```

2. Write German content to `DE/asciidoc/src/08_crosscutting/08_5_caching.adoc`:
   ```asciidoc
   === 8.5 Caching-Strategie

   ==== Motivation
   Redis wird als zentraler Cache für häufig abgerufene Daten verwendet...

   ==== Implementierung
   [Deutscher Inhalt hier]
   ```

3. Write English content to `EN/asciidoc/src/08_crosscutting/08_5_caching.adoc`:
   ```asciidoc
   === 8.5 Caching Strategy

   ==== Motivation
   Redis is used as a central cache for frequently accessed data...

   ==== Implementation
   [English content here]
   ```

4. Update BOTH `arc42-template.adoc` files (DE and EN) to include the new section:
   ```asciidoc
   // In section "Kapitel 8" (DE) or "Chapter 8" (EN)
   include::src/08_crosscutting/08_5_caching.adoc[]
   ```

5. Test build:
   ```bash
   ./gradlew buildAll
   ```

6. Commit with descriptive message:
   ```bash
   git add .
   git commit -m "docs: Add Redis caching strategy to chapter 8

   - Added section 8.5 in both DE and EN
   - Documented caching approach, TTL settings, and invalidation strategy"
   ```

### Task 2: Update Existing Content

**User Request:** "Update the Stripe integration to mention Apple Pay support"

**Steps:**
1. Edit `DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:
   - Add German text about Apple Pay

2. Edit `EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:
   - Add corresponding English text about Apple Pay

3. If version change is significant, update `version.properties` in BOTH directories:
   ```properties
   # DE/asciidoc/version.properties and EN/asciidoc/version.properties
   version=1.1
   build.date=2025-11-14
   language=DE  (or EN for English file)
   ```

4. Test and commit:
   ```bash
   ./gradlew buildAll
   git add .
   git commit -m "docs: Add Apple Pay support to Stripe integration documentation"
   ```

### Task 3: Add Screenshots

**User Request:** "Add screenshots of the admin dashboard"

**Steps:**
1. Obtain screenshots in both languages:
   - German UI screenshot → `admin-dashboard-de.png`
   - English UI screenshot → `admin-dashboard-en.png`

2. Place in correct directories:
   ```bash
   cp /path/to/screenshot-de.png images/DE/admin-dashboard-de.png
   cp /path/to/screenshot-en.png images/EN/admin-dashboard-en.png
   ```

3. Reference in DE document:
   ```asciidoc
   // In DE/asciidoc/src/05_2_frontend.adoc
   ==== Admin Dashboard

   Das Admin-Dashboard zeigt alle wichtigen Metriken auf einen Blick.

   image::DE/admin-dashboard-de.png[Admin Dashboard,800,align=center]
   ```

4. Reference in EN document:
   ```asciidoc
   // In EN/asciidoc/src/05_2_frontend.adoc
   ==== Admin Dashboard

   The admin dashboard displays all important metrics at a glance.

   image::EN/admin-dashboard-en.png[Admin Dashboard,800,align=center]
   ```

5. Commit:
   ```bash
   git add images/ DE/ EN/
   git commit -m "docs: Add admin dashboard screenshots to frontend section"
   ```

### Task 4: Add PlantUML Diagram

**User Request:** "Add a sequence diagram for the payment flow"

**Steps:**
1. Embed in BOTH DE and EN documents (adjust text for language):

   **German** (`DE/asciidoc/src/06_runtime/06_2_payment_flow.adoc`):
   ```asciidoc
   [plantuml,payment-sequence-de,png]
   ----
   @startuml
   actor Kunde
   participant "Frontend" as FE
   participant "Backend" as BE
   participant "Stripe" as Stripe

   Kunde -> FE: Zahlung initiieren
   FE -> BE: POST /api/payment
   BE -> Stripe: Create Payment Intent
   Stripe -> BE: Payment Intent ID
   @enduml
   ----
   ```

   **English** (`EN/asciidoc/src/06_runtime/06_2_payment_flow.adoc`):
   ```asciidoc
   [plantuml,payment-sequence-en,png]
   ----
   @startuml
   actor Customer
   participant "Frontend" as FE
   participant "Backend" as BE
   participant "Stripe" as Stripe

   Customer -> FE: Initiate payment
   FE -> BE: POST /api/payment
   BE -> Stripe: Create Payment Intent
   Stripe -> BE: Payment Intent ID
   @enduml
   ----
   ```

2. Note: Use different diagram IDs (`payment-sequence-de` vs `payment-sequence-en`) if text differs
3. Use same ID and place in `images/shared/` if diagram has no language-specific text

### Task 5: Create Nested Subfolder Structure

**User Request:** "Add a new integrations section with subfolders for each integration"

**Steps:**
1. Create folder structure in BOTH languages:
   ```bash
   mkdir -p DE/asciidoc/src/05_building_blocks/05_4_integrations
   mkdir -p EN/asciidoc/src/05_building_blocks/05_4_integrations
   ```

2. Create files in BOTH directories:
   ```bash
   touch DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc
   touch EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc
   ```

3. Add content to each file (German to DE, English to EN)

4. Update main template to include new files

---

## Build System

### Gradle Tasks

**Key Commands:**

```bash
# Build everything (HTML + PDF for both DE and EN)
./gradlew buildAll

# Build only German documentation
./gradlew asciidoctorHtmlDE asciidoctorPdfDE

# Build only English documentation
./gradlew asciidoctorHtmlEN asciidoctorPdfEN

# Clean build artifacts
./gradlew clean

# Clean and rebuild
./gradlew clean buildAll
```

### Build Output Locations

After running `./gradlew buildAll`:

```
build/
├── DE/
│   ├── html/
│   │   └── arc42-template.html
│   └── pdf/
│       └── arc42-template.pdf
└── EN/
    ├── html/
    │   └── arc42-template.html
    └── pdf/
        └── arc42-template.pdf
```

### Build Configuration

**File:** `build.gradle`

- Asciidoctor plugins: v4.0.2
- AsciidoctorJ: v2.5.13
- Backends: HTML5, PDF
- Dynamic language support: Reads from `gradle.properties`
- Image directory: `../../images` (relative to `.adoc` source files)

**File:** `gradle.properties`

```properties
languages=DE,EN
org.gradle.parallel=true
org.gradle.caching=true
```

---

## CI/CD Pipeline

### GitHub Actions Workflow

**File:** `.github/workflows/check-symmetry.yml`

**Triggers:**
- Pull requests affecting `DE/asciidoc/**`, `EN/asciidoc/**`, or `images/**`
- Pushes to `main` branch

**Jobs:**

#### 1. `symmetry-check`
- Validates file count: `DE/` and `EN/` have same number of `.adoc` files
- Validates filenames: All basenames match between languages
- Validates folder structure: Directory trees are identical

**Example output:**
```
✓ Struktur symmetrisch: 24 Dateien in beiden Sprachen
✓ Dateinamen identisch
✓ Ordnerstruktur symmetrisch
```

#### 2. `build-documentation`
- Depends on: `symmetry-check` (won't run if symmetry fails)
- Sets up Java 17
- Runs `./gradlew buildAll`
- Uploads build artifacts (HTML and PDF for both languages)

**Viewing artifacts:**
1. Go to repository → Actions tab
2. Click on workflow run
3. Download "documentation" artifact

### Pull Request Template

**File:** `.github/PULL_REQUEST_TEMPLATE.md`

**Key Sections:**
- Component/Feature overview
- Symmetry checks (MUST HAVE):
  - [ ] New `.adoc` files in BOTH `DE/` and `EN/`?
  - [ ] Folder structure identical?
  - [ ] All image paths relative?
  - [ ] Same structure in both language files?
  - [ ] `version.properties` updated in DE and EN?
- Content checks (grammar, images)
- Format checks (AsciiDoc syntax, commit messages)

**As an AI assistant, mentally review this checklist before committing.**

---

## Git Workflow for AI Assistants

### Branch Strategy

- **Main branch:** `main` (protected)
- **Feature branches:** Use descriptive names (e.g., `feature/add-redis-caching`, `docs/update-stripe-integration`)
- **Current branch:** `claude/update-claude-md-01QGqdud5FowswH9D3fktibA`

### Commit Message Format

Use conventional commit format:

```
docs: <short description>

<optional longer description>
<optional bullet points>
```

**Examples:**
- `docs: Add Redis caching strategy to chapter 8`
- `docs: Update Stripe integration with Apple Pay support`
- `docs: Fix image paths in deployment chapter`
- `fix: Correct symmetry in chapter 5 filenames`

### Typical Workflow

```bash
# 1. Verify you're on the correct branch
git status

# 2. Make changes to BOTH DE/ and EN/ files

# 3. Test build locally
./gradlew buildAll

# 4. Check what changed
git status
git diff

# 5. Stage all changes
git add .

# 6. Commit with descriptive message
git commit -m "docs: Add new integration documentation

- Added section 5.4.4 for Salesforce integration
- Included sequence diagrams and error handling tables
- Updated both DE and EN versions"

# 7. Push to remote (use -u if first push)
git push -u origin <branch-name>

# 8. If push fails due to network, retry with exponential backoff
# (automatically handled in this environment)
```

---

## AsciiDoc Conventions

### Document Structure

**Main template** (`arc42-template.adoc`):
```asciidoc
= Arc42 Architekturdokumentation: B2B E-Commerce Data Pipeline
:doctype: book
:sectnums:
:sectanchors:
:toc: left
:toclevels: 3
:icons: font
:imagesdir: ../../images
:source-highlighter: highlight.js

include::version.properties[]

[.text-right]
_Version {version}, Build-Datum: {build.date}, Sprache: {language}_

<<<

// Kapitel 1
include::src/01_introduction.adoc[]

<<<

// Kapitel 2
include::src/02_constraints.adoc[]
```

### Chapter Files

**Pattern:**
```asciidoc
== 1. Einführung und Ziele

=== 1.1 Aufgabenstellung

Das System ermöglicht...

**Kernfunktionen:**

* Funktion 1
* Funktion 2

=== 1.2 Qualitätsziele

[cols="1,2,3",options="header"]
|===
|Priorität |Qualitätsziel |Motivation
|1 |Sicherheit |...
|===
```

### Headings

- Level 2 (`==`): Chapter numbers (e.g., `== 1. Einführung`)
- Level 3 (`===`): Sections (e.g., `=== 1.1 Aufgabenstellung`)
- Level 4 (`====`): Subsections (e.g., `==== 1.1.1 Scope`)

### Tables

```asciidoc
[cols="1,2,3",options="header"]
|===
|Column 1 |Column 2 |Column 3

|Data 1
|Data 2
|Data 3
|===
```

### Code Blocks

```asciidoc
[source,java]
----
public class Example {
    // Code here
}
----
```

### Lists

**Unordered:**
```asciidoc
* Item 1
* Item 2
* Item 3
```

**Ordered:**
```asciidoc
. First
. Second
. Third
```

### Page Breaks

```asciidoc
<<<
```

---

## Important Files to Know

### Configuration Files

| File | Location | Purpose |
|------|----------|---------|
| `arc42-template.adoc` | `DE/asciidoc/` and `EN/asciidoc/` | Main template, includes all chapters |
| `version.properties` | `DE/asciidoc/` and `EN/asciidoc/` | Version metadata (v1.0, build date, language) |
| `build.gradle` | Root | Gradle build configuration |
| `gradle.properties` | Root | Build properties (languages, parallel build) |
| `settings.gradle` | Root | Project name |
| `.gitignore` | Root | Git ignore rules (excludes `build/`, `.gradle/`, IDE files) |

### Documentation Files

| File | Purpose |
|------|---------|
| `README.md` | User-facing quick start and reference |
| `ARCHITECTURE_DOCS_GUIDE.md` | Comprehensive 700+ line editing guide |
| `CLAUDE.md` | This file - AI assistant guide |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR checklist |

### Workflow Files

| File | Purpose |
|------|---------|
| `.github/workflows/check-symmetry.yml` | CI/CD pipeline |

---

## Testing and Validation

### Local Testing Checklist

Before committing, ALWAYS:

1. **Build documentation:**
   ```bash
   ./gradlew buildAll
   ```

2. **Check for errors:**
   - Look for AsciiDoc syntax errors in build output
   - Watch for "include file not found" errors
   - Check for missing images warnings

3. **Verify output:**
   ```bash
   # Open generated HTML files
   open build/DE/html/arc42-template.html
   open build/EN/html/arc42-template.html
   ```

4. **Check structure:**
   - Table of contents shows all chapters?
   - Images display correctly?
   - Cross-references work?

5. **Validate symmetry manually:**
   ```bash
   # Count files
   find DE/asciidoc/src -name "*.adoc" | wc -l
   find EN/asciidoc/src -name "*.adoc" | wc -l
   # Should be equal

   # Compare filenames
   diff <(find DE/asciidoc/src -name "*.adoc" -exec basename {} \; | sort) \
        <(find EN/asciidoc/src -name "*.adoc" -exec basename {} \; | sort)
   # Should show no differences
   ```

### CI/CD Validation

The CI pipeline will automatically:
- ✅ Validate symmetry (file count, filenames, folders)
- ✅ Build documentation for both languages
- ✅ Upload artifacts

If CI fails:
1. Check the Actions tab in GitHub
2. Read the error message
3. Fix locally
4. Push again

---

## Common Pitfalls and How to Avoid Them

### ❌ Pitfall 1: Creating File in Only One Language

**Problem:**
```bash
# Only create German file
touch DE/asciidoc/src/09_1_new_decision.adoc
```

**Result:** CI fails with "Asymmetrische Struktur: DE=25, EN=24"

**Solution:**
```bash
# ALWAYS create in BOTH languages
touch DE/asciidoc/src/09_1_new_decision.adoc
touch EN/asciidoc/src/09_1_new_decision.adoc
```

### ❌ Pitfall 2: Incorrect Image Paths

**Problem:**
```asciidoc
image::/images/shared/diagram.png[]  # Absolute path - WRONG
```

**Result:** Image not found in generated output

**Solution:**
```asciidoc
image::shared/diagram.png[]  # Relative to :imagesdir: - CORRECT
# OR
image::../../images/shared/diagram.png[]  # Full relative path - ALSO CORRECT
```

### ❌ Pitfall 3: Forgetting to Include New Files in Template

**Problem:**
- Create `08_5_caching.adoc` in both languages
- Forget to add `include::` statement to `arc42-template.adoc`

**Result:** Content exists but doesn't appear in generated documentation

**Solution:**
- **ALWAYS** update both `DE/asciidoc/arc42-template.adoc` and `EN/asciidoc/arc42-template.adoc`
- Add: `include::src/08_crosscutting/08_5_caching.adoc[]`

### ❌ Pitfall 4: Using Language-Specific Images in Wrong Folder

**Problem:**
```asciidoc
# Screenshot has German UI text, but placed in shared/
image::shared/admin-screenshot.png[]
```

**Result:** English documentation shows German UI

**Solution:**
- German UI screenshots → `images/DE/`
- English UI screenshots → `images/EN/`
- Reference correctly in each language version

### ❌ Pitfall 5: Forgetting to Update version.properties

**Problem:**
- Make significant changes to documentation
- Don't update version number or build date

**Result:** Version metadata is stale

**Solution:**
- Update `DE/asciidoc/version.properties` AND `EN/asciidoc/version.properties`
- Increment version (1.0 → 1.1) for minor changes, (1.0 → 2.0) for major changes
- Update `build.date` to current date

### ❌ Pitfall 6: Creating Empty Files Without Content

**Problem:**
```bash
# Create files but leave them empty
touch DE/asciidoc/src/08_5_caching.adoc
touch EN/asciidoc/src/08_5_caching.adoc
# Commit without adding content
```

**Result:** Documentation builds but has empty sections

**Solution:**
- **ALWAYS** add content (even placeholder/TODO content) to new files
- Minimum content:
  ```asciidoc
  === 8.5 Caching Strategy

  [TODO: Document Redis caching implementation]
  ```

---

## Quick Reference

### Essential Commands

```bash
# Build everything
./gradlew buildAll

# Build only HTML (faster for testing)
./gradlew asciidoctorHtmlDE asciidoctorHtmlEN

# Clean and rebuild
./gradlew clean buildAll

# View build output
open build/DE/html/arc42-template.html
open build/EN/html/arc42-template.html

# Check symmetry manually
diff <(find DE/asciidoc/src -name "*.adoc" -exec basename {} \; | sort) \
     <(find EN/asciidoc/src -name "*.adoc" -exec basename {} \; | sort)

# Git workflow
git status
git add .
git commit -m "docs: Your message here"
git push -u origin <branch-name>
```

### File Count Quick Check

```bash
echo "DE files: $(find DE/asciidoc/src -name '*.adoc' | wc -l)"
echo "EN files: $(find EN/asciidoc/src -name '*.adoc' | wc -l)"
# Should show: 24 files each (as of 2025-11-14)
```

### Image Reference Templates

```asciidoc
# Shared image (same in DE and EN)
image::shared/architecture-diagram.png[Description,800,align=center]

# German-specific image (in DE document)
image::DE/screenshot-de.png[Description,600,align=center]

# English-specific image (in EN document)
image::EN/screenshot-en.png[Description,600,align=center]
```

---

## Decision-Making Framework for AI Assistants

When users request changes, follow this decision tree:

### Step 1: Identify Impact

**Question:** Does this change affect file structure (create, delete, rename)?

- ✅ YES → Perform operation in BOTH `DE/` and `EN/`
- ❌ NO → Proceed to Step 2

### Step 2: Identify Content Type

**Question:** Is this a content-only change (text within existing files)?

- ✅ YES → Update both language versions with appropriate translated content
- ❌ NO → Proceed to Step 3

### Step 3: Identify Image Impact

**Question:** Does this involve images?

- ✅ YES → Determine category:
  - No text in image → `images/shared/`
  - German text → `images/DE/`
  - English text → `images/EN/`
- ❌ NO → Proceed to Step 4

### Step 4: Build and Validate

1. Run `./gradlew buildAll`
2. Check for errors
3. Verify output in browser
4. Commit with descriptive message

---

## Project Context

### Business Domain

**B2B E-Commerce Data Pipeline** for mid-sized companies with:
- Order management with volume discounts
- Payment processing (Stripe integration)
- Marketplace synchronization (Amazon Business, Mercateo)
- ERP integration (SAP, Microsoft Dynamics)
- Real-time analytics dashboards

### Quality Goals (Priority Order)

1. **Security:** PCI-DSS compliance, GDPR compliance
2. **Availability:** 99.5% uptime, 24/7 operations
3. **Performance:** <500ms API response time, handle 50K orders/day
4. **Integrability:** REST APIs, webhook support, multiple ERP systems
5. **Maintainability:** CI/CD, modular architecture, comprehensive tests
6. **Print Optimization:** Business documents (orders, invoices) must be printable with high readability on physical media, DIN A4 format, min 10pt font, @media print CSS rules

### Arc42 Chapter Mapping

| Chapter | German | English | Content |
|---------|--------|---------|---------|
| 1 | Einführung und Ziele | Introduction and Goals | Business context, quality goals |
| 2 | Randbedingungen | Constraints | Technical/organizational constraints |
| 3 | Kontextabgrenzung | Context and Scope | System boundaries, external interfaces |
| 4 | Lösungsstrategie | Solution Strategy | Key architectural decisions |
| 5 | Bausteinsicht | Building Blocks | Component structure (frontend, backend, integrations) |
| 5.4.1 | Stripe-Integration | Stripe Integration | Payment processing details |
| 5.4.2 | Marketplace-Integration | Marketplace Integration | B2B marketplace connections |
| 5.4.3 | ERP-Integration | ERP Integration | SAP/Dynamics integration |
| 6 | Laufzeitsicht | Runtime View | Order flow, payment flow, integration flow |
| 7 | Verteilung | Deployment | Infrastructure and deployment |
| 8 | Querschnittliche Konzepte | Crosscutting Concepts | Logging, error handling, auth, API conventions |
| 9 | Entscheidungen | Decisions | ADRs (Architecture Decision Records) |
| 10 | Qualität | Quality | Quality requirements and scenarios |
| 11 | Risiken | Risks | Technical and business risks |
| 12 | Glossar | Glossary | Term definitions |

---

## Summary: The AI Assistant Pledge

As an AI assistant working in this repository, I pledge to:

1. ✅ **ALWAYS** maintain DE/EN symmetry when creating, modifying, or deleting files
2. ✅ **ALWAYS** create files in BOTH `DE/` and `EN/` directories simultaneously
3. ✅ **ALWAYS** use identical filenames and folder structures
4. ✅ **ALWAYS** use relative image paths or respect the `:imagesdir:` attribute
5. ✅ **ALWAYS** place images in the correct category (`shared/`, `DE/`, or `EN/`)
6. ✅ **ALWAYS** build documentation locally before committing (`./gradlew buildAll`)
7. ✅ **ALWAYS** update `version.properties` in both languages when making significant changes
8. ✅ **ALWAYS** include new files in both `arc42-template.adoc` templates
9. ✅ **ALWAYS** use descriptive commit messages following conventional commit format
10. ✅ **ALWAYS** verify output by checking generated HTML/PDF files

**When in doubt, maintain symmetry. It's better to ask than to break the build.**

---

## Additional Resources

- **README.md**: Quick start guide for users
- **ARCHITECTURE_DOCS_GUIDE.md**: Comprehensive 700+ line guide with detailed workflows
- **Arc42 Template Documentation**: https://arc42.org/
- **AsciiDoc Syntax**: https://docs.asciidoctor.org/asciidoc/latest/
- **Gradle Build**: https://docs.gradle.org/

---

**Last Updated:** 2025-11-14
**Repository Version:** 1.0
**Branch:** `claude/update-claude-md-01QGqdud5FowswH9D3fktibA`
