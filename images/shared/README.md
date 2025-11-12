# Shared Images

This directory contains **language-independent** images, diagrams, and visualizations.

## Usage

Use images from this directory when:
- The diagram contains no text
- The diagram contains only technical terms (no translation needed)
- The image is a purely visual representation (architecture diagrams, flowcharts, etc.)

## Naming Convention

`{component}-{purpose}.{format}`

Examples:
- `architecture-overview.drawio.png`
- `c4-deployment.drawio.png`
- `database-schema.drawio.png`
- `stripe-payment-flow.drawio.png`

## Reference in AsciiDoc

Both DE and EN documents reference these files identically:

```asciidoc
image::shared/architecture-overview.drawio.png[Architecture Overview,800,align=center]
```

## Tools

Recommended tools for creating diagrams:
- **Draw.io / diagrams.net**: For architecture and flowchart diagrams
- **PlantUML**: For UML diagrams (embedded in AsciiDoc)
- **Mermaid**: For simple diagrams (embedded in AsciiDoc)
- **dbdiagram.io**: For database schemas
