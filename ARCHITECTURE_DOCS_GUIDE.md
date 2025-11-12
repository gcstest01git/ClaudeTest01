# Architecture Documentation Guide

Comprehensive guide for maintaining multilingual arc42 architecture documentation with strict DE/EN symmetry governance.

## Table of Contents

1. [Core Principles](#core-principles)
2. [Structure & Symmetry](#structure--symmetry)
3. [Image Management](#image-management)
4. [Detailed Workflows](#detailed-workflows)
5. [Git Hooks & CI/CD](#git-hooks--cicd)
6. [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)

---

## Core Principles

### The Symmetry Rule

**Definition**: Every structural element (file, folder) in `DE/` must have an exact counterpart in `EN/` with identical:
- Filename
- Path structure
- Folder hierarchy
- File type

**Only the text content differs between languages.**

### Why Symmetry Matters

1. **Consistency**: Ensures both language versions are complete
2. **Maintainability**: Easy to track missing translations
3. **Quality**: Automated validation prevents incomplete documentation
4. **Governance**: Enforced by git hooks and CI/CD

---

## Structure & Symmetry

### Directory Layout

```
<root>/
├── DE/asciidoc/src/          # German content
├── EN/asciidoc/src/          # English content (MUST mirror DE/)
└── images/
    ├── shared/               # Language-independent diagrams
    ├── DE/                   # German screenshots
    └── EN/                   # English screenshots
```

### File Naming Convention

**Pattern**: `{chapter_number}_{topic}.adoc`

Examples:
- `01_introduction.adoc`
- `05_building_blocks.adoc`
- `05_4_1_stripe.adoc`

**Rules**:
- Use lowercase
- Use underscores, not hyphens
- Include chapter numbers for ordering
- Be descriptive but concise

### Folder Structure for Subchapters

For complex chapters with multiple sections:

```
05_building_blocks/
├── 05_1_overview.adoc
├── 05_2_frontend.adoc
├── 05_3_backend.adoc
├── 05_4_integrations.adoc           # Overview
├── 05_4_integrations/
│   ├── 05_4_1_stripe.adoc
│   ├── 05_4_2_marketplace.adoc
│   └── 05_4_3_erp.adoc
└── 05_5_data_layer.adoc
```

**This exact structure must exist in both DE/ and EN/.**

---

## Image Management

### Three Image Categories

#### 1. Shared Images (`images/shared/`)

**Use for**: Technical diagrams, architecture overviews, database schemas, flowcharts without language-specific text.

**Examples**:
- `architecture-overview.drawio.png`
- `c4-deployment.drawio.png`
- `database-schema.drawio.png`
- `stripe-payment-flow.drawio.png`

**AsciiDoc Reference** (same in both DE and EN):
```asciidoc
image::../../images/shared/architecture-overview.drawio.png[Architecture Overview,800,align=center]
```

#### 2. German Images (`images/DE/`)

**Use for**: Screenshots with German UI text, diagrams with German labels.

**Examples**:
- `order-management-de.png`
- `stripe-checkout-de.png`
- `admin-panel-de.png`
- `stripe-error-handling-de.png`

**AsciiDoc Reference** (in DE documents):
```asciidoc
image::../../images/DE/stripe-checkout-de.png[Stripe Checkout,600,align=center]
```

#### 3. English Images (`images/EN/`)

**Use for**: Screenshots with English UI text, diagrams with English labels.

**Examples**:
- `order-management-en.png`
- `stripe-checkout-en.png`
- `admin-panel-en.png`
- `stripe-error-handling-en.png`

**AsciiDoc Reference** (in EN documents):
```asciidoc
image::../../images/EN/stripe-checkout-en.png[Stripe Checkout,600,align=center]
```

### Image Path Rules

**ALWAYS use relative paths from the `.adoc` file location:**

From `DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:
```asciidoc
image::../../../../images/DE/stripe-checkout-de.png[]
```

**Or configure `imagesdir` in the main template** (recommended):
```asciidoc
// In arc42-template.adoc
:imagesdir: ../../images

// Then in any included file
image::DE/stripe-checkout-de.png[]
```

### Naming Convention for Images

**Pattern**: `{component}-{purpose}-{language}.png`

Examples:
- `stripe-checkout-de.png` / `stripe-checkout-en.png`
- `order-flow-diagram.png` (shared, no language suffix)
- `admin-dashboard-de.png` / `admin-dashboard-en.png`

---

## Detailed Workflows

### Workflow 1: Adding a New Integration (Example: Stripe)

**Scenario**: Document a new payment integration with Stripe.

#### Step 1: Create Files in Both Languages

```bash
# Create German document
touch DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc

# Create English document
touch EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc
```

#### Step 2: Add Content to German Document

`DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:

```asciidoc
==== 5.4.1 Stripe Payment Integration

===== Zweck

Integration der Stripe-Zahlungsplattform für sichere Kreditkarten- und Wallet-Zahlungen.

===== Zuständigkeiten

* Zahlungsabwicklung (Kreditkarten, Apple Pay, Google Pay)
* Tokenisierung sensibler Kartendaten
* Webhook-Verarbeitung für Zahlungsstatus
* Rückerstattungsmanagement

===== Schnittstellen

[plantuml,stripe-payment-sequence,png]
----
@startuml
actor Customer
participant "Frontend" as FE
participant "Backend API" as BE
participant "Stripe API" as Stripe

Customer -> FE: Checkout initiieren
FE -> BE: POST /api/checkout/session
BE -> Stripe: Create Checkout Session
Stripe -> BE: Session ID + URL
BE -> FE: Checkout URL
FE -> Customer: Redirect zu Stripe
Customer -> Stripe: Zahlung durchführen
Stripe -> BE: Webhook: payment_intent.succeeded
BE -> BE: Bestellung bestätigen
@enduml
----

image::../../../../images/shared/stripe-payment-flow.drawio.png[Stripe Payment Flow,800,align=center]

===== Fehlerbehandlung

[cols="2,3,3",options="header"]
|===
|Fehlertyp |Ursache |Maßnahme

|`payment_failed`
|Karte abgelehnt
|Benutzer informieren, alternative Zahlungsmethode anbieten

|`webhook_timeout`
|Webhook nicht empfangen
|Manuelles Polling des Payment Status nach 5 Min.

|`invalid_api_key`
|Stripe API-Schlüssel ungültig
|Alert an DevOps, Fallback zu Wartungsmodus
|===

image::../../../../images/DE/stripe-error-handling-de.png[Fehlerbehandlung,600,align=center]
```

#### Step 3: Add Content to English Document

`EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:

```asciidoc
==== 5.4.1 Stripe Payment Integration

===== Purpose

Integration of the Stripe payment platform for secure credit card and wallet payments.

===== Responsibilities

* Payment processing (credit cards, Apple Pay, Google Pay)
* Tokenization of sensitive card data
* Webhook processing for payment status
* Refund management

===== Interfaces

[plantuml,stripe-payment-sequence-en,png]
----
@startuml
actor Customer
participant "Frontend" as FE
participant "Backend API" as BE
participant "Stripe API" as Stripe

Customer -> FE: Initiate checkout
FE -> BE: POST /api/checkout/session
BE -> Stripe: Create Checkout Session
Stripe -> BE: Session ID + URL
BE -> FE: Checkout URL
FE -> Customer: Redirect to Stripe
Customer -> Stripe: Complete payment
Stripe -> BE: Webhook: payment_intent.succeeded
BE -> BE: Confirm order
@enduml
----

image::../../../../images/shared/stripe-payment-flow.drawio.png[Stripe Payment Flow,800,align=center]

===== Error Handling

[cols="2,3,3",options="header"]
|===
|Error Type |Cause |Action

|`payment_failed`
|Card declined
|Notify user, offer alternative payment method

|`webhook_timeout`
|Webhook not received
|Manual polling of payment status after 5 min.

|`invalid_api_key`
|Stripe API key invalid
|Alert DevOps, fallback to maintenance mode
|===

image::../../../../images/EN/stripe-error-handling-en.png[Error Handling,600,align=center]
```

#### Step 4: Include in Main Template

In `DE/asciidoc/arc42-template.adoc` and `EN/asciidoc/arc42-template.adoc`:

```asciidoc
=== 5.4 Externe Integrationen
include::src/05_building_blocks/05_4_integrations.adoc[]
include::src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc[]
include::src/05_building_blocks/05_4_integrations/05_4_2_marketplace.adoc[]
include::src/05_building_blocks/05_4_integrations/05_4_3_erp.adoc[]
```

#### Step 5: Add Images

Create or copy:
- `images/shared/stripe-payment-flow.drawio.png` (technical flow, no language)
- `images/DE/stripe-error-handling-de.png` (German UI screenshot)
- `images/EN/stripe-error-handling-en.png` (English UI screenshot)

#### Step 6: Test Build

```bash
./gradlew buildAll

# Check output
open build/DE/html/arc42-template.html
open build/EN/html/arc42-template.html
```

#### Step 7: Commit

```bash
git add .
git commit -m "docs: Add Stripe payment integration documentation

- Added section 5.4.1 in both DE and EN
- Included PlantUML sequence diagram
- Added error handling table
- Added German and English screenshots"
```

---

### Workflow 2: Updating Existing Documentation

**Scenario**: Add a new error scenario to the Stripe integration.

#### Step 1: Edit German Document

Add row to error table in `DE/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:

```asciidoc
|`rate_limit_exceeded`
|Zu viele API-Anfragen
|Exponentielles Backoff, max. 3 Versuche
```

#### Step 2: Edit English Document

Add corresponding row to `EN/asciidoc/src/05_building_blocks/05_4_integrations/05_4_1_stripe.adoc`:

```asciidoc
|`rate_limit_exceeded`
|Too many API requests
|Exponential backoff, max. 3 retries
```

#### Step 3: Update Version

Edit `DE/asciidoc/version.properties`:
```properties
version=1.1
build.date=2025-11-12
language=DE
```

Edit `EN/asciidoc/version.properties`:
```properties
version=1.1
build.date=2025-11-12
language=EN
```

#### Step 4: Test & Commit

```bash
./gradlew buildAll
git add .
git commit -m "docs: Add rate limiting error scenario to Stripe integration"
```

---

### Workflow 3: Adding Screenshots

**Scenario**: Document the admin dashboard with screenshots.

#### Step 1: Take Screenshots

Take screenshots in both languages:
- German UI: Save as `admin-dashboard-de.png`
- English UI: Save as `admin-dashboard-en.png`

#### Step 2: Place Files

```bash
cp /path/to/admin-dashboard-de.png images/DE/
cp /path/to/admin-dashboard-en.png images/EN/
```

#### Step 3: Reference in Documents

In `DE/asciidoc/src/05_2_frontend.adoc`:
```asciidoc
==== Admin Dashboard

Das Admin-Dashboard bietet eine Übersicht über alle wichtigen Metriken.

image::../../images/DE/admin-dashboard-de.png[Admin Dashboard,800,align=center]
```

In `EN/asciidoc/src/05_2_frontend.adoc`:
```asciidoc
==== Admin Dashboard

The admin dashboard provides an overview of all important metrics.

image::../../images/EN/admin-dashboard-en.png[Admin Dashboard,800,align=center]
```

---

## Git Hooks & CI/CD

### Pre-Commit Hook

**Location**: `.git/hooks/pre-commit`

**What it checks**:
1. File count: `DE/` and `EN/` have same number of `.adoc` files
2. Filenames: All filenames match between languages
3. Folder structure: Directory trees are identical

**When it runs**: Before every `git commit`

**If it fails**:
```
❌ ERROR: DE und EN haben unterschiedliche Dateizahl!
   DE: 25 Dateien
   EN: 24 Dateien
```

**Action**: Fix the asymmetry, then commit again.

### GitHub Actions Workflow

**Location**: `.github/workflows/check-symmetry.yml`

**Triggers**:
- Pull requests affecting documentation
- Pushes to `main` branch

**Jobs**:
1. **symmetry-check**: Validates DE/EN structure
2. **build-documentation**: Builds HTML and PDF, uploads artifacts

**View results**: Go to repository → Actions tab

---

## Best Practices

### 1. Always Work in Pairs (DE + EN)

Never create or edit a file in only one language. Always update both.

### 2. Use Consistent Structure

Both language versions should have:
- Same headings (different language, same hierarchy)
- Same tables (same columns)
- Same diagrams (reference same files or localized versions)

### 3. Shared vs. Localized Images

**Rule of thumb**:
- No text in image → `shared/`
- English text → `EN/`
- German text → `DE/`
- Technical terms only → `shared/`

### 4. Version Properties

Update `version.properties` in **both** languages when making significant changes:
- Major changes: Increment major version (1.x → 2.0)
- Minor updates: Increment minor version (1.0 → 1.1)
- Always update `build.date`

### 5. Commit Messages

Use conventional commit format:
- `docs: Add API authentication chapter`
- `docs: Update Stripe integration diagram`
- `fix: Correct image path in deployment chapter`

### 6. PlantUML Diagrams

Embed diagrams directly in `.adoc` files for version control:

```asciidoc
[plantuml,diagram-name,png]
----
@startuml
' Your PlantUML code here
@enduml
----
```

Or reference external files:
```asciidoc
plantuml::diagrams/architecture.puml[format=png]
```

### 7. Table Formatting

Use AsciiDoc table syntax:

```asciidoc
[cols="1,2,3",options="header"]
|===
|Column 1 |Column 2 |Column 3

|Data 1
|Data 2
|Data 3
|===
```

---

## Troubleshooting

### Problem: Pre-commit hook rejected my commit

**Symptoms**:
```
❌ ERROR: Dateinamen unterscheiden sich zwischen DE und EN!
```

**Solution**:
1. Check which files are missing:
   ```bash
   diff <(find DE/asciidoc/src -name "*.adoc" | xargs -I {} basename {}) \
        <(find EN/asciidoc/src -name "*.adoc" | xargs -I {} basename {})
   ```

2. Create missing files or rename to match

### Problem: Build fails with "include file not found"

**Symptoms**:
```
ERROR: include file not found: src/05_4_1_stripe.adoc
```

**Solution**:
1. Check that the file exists at the path specified
2. Verify `include::` path is relative to the file containing it
3. Ensure no typos in filename

### Problem: Images not appearing in generated HTML

**Symptoms**: Image placeholder or broken image icon in output

**Solution**:
1. Verify image file exists at specified path
2. Check path is relative: `../../images/shared/diagram.png`
3. Ensure `imagesdir` attribute is set correctly in main template:
   ```asciidoc
   :imagesdir: ../../images
   ```

### Problem: GitHub Actions failing on symmetry check

**Symptoms**: CI pipeline shows red ❌ for symmetry-check job

**Solution**:
1. Go to Actions tab, click on failed workflow
2. Read the error message to identify which files are asymmetric
3. Fix locally:
   ```bash
   # Create missing file
   touch EN/asciidoc/src/{missing-file}.adoc

   # Commit and push
   git add .
   git commit -m "fix: Add missing EN translation file"
   git push
   ```

### Problem: Different line endings (CRLF vs LF)

**Symptoms**: Git shows all files as modified on Windows

**Solution**:
Configure git to auto-convert line endings:
```bash
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input # Mac/Linux
```

### Problem: Gradle build is slow

**Solution**:
1. Use parallel builds (already enabled in `gradle.properties`):
   ```properties
   org.gradle.parallel=true
   org.gradle.caching=true
   ```

2. Build only one language for quick tests:
   ```bash
   ./gradlew asciidoctorHtmlDE
   ```

3. Use Gradle daemon (enabled by default)

---

## Advanced Topics

### Custom AsciiDoc Attributes

Define custom attributes in main template:

```asciidoc
:company: B2B Commerce Inc.
:version: 1.0
:project-name: Data Pipeline

= Arc42 Documentation: {project-name}
Version {version}
```

### Cross-References

Link between chapters:

```asciidoc
// In any document
See <<05_4_1_stripe.adoc#,Stripe Integration>> for payment details.
```

### Conditional Content

Show content only in specific builds:

```asciidoc
ifdef::html-backend[]
HTML-only content here
endif::[]

ifdef::pdf-backend[]
PDF-only content here
endif::[]
```

### Including External Files

Include code snippets from actual source code:

```asciidoc
[source,java]
----
include::../../src/main/java/PaymentService.java[lines=10..30]
----
```

---

## Summary Checklist

Before committing documentation changes:

- [ ] Both DE and EN files created/updated?
- [ ] Same folder structure in both languages?
- [ ] All images exist at specified paths?
- [ ] Language-specific images in correct folders (DE/ or EN/)?
- [ ] Build successful? (`./gradlew buildAll`)
- [ ] HTML output checked for both languages?
- [ ] version.properties updated (if needed)?
- [ ] Commit message follows convention?

Before creating a pull request:

- [ ] Pre-commit hook passed?
- [ ] All items in PR template checklist completed?
- [ ] CI pipeline green?
- [ ] Documentation reviewed for spelling/grammar?

---

**For questions or issues, refer to README.md or open a GitHub issue.**
