# English Images

This directory contains **English-language** screenshots, diagrams, and visualizations.

## Usage

Use images from this directory when:
- Screenshots contain English UI text
- Diagrams have English labels
- Explanations are in English

## Naming Convention

`{component}-{purpose}-en.{format}`

Examples:
- `order-management-en.png`
- `stripe-checkout-en.png`
- `admin-panel-en.png`
- `stripe-error-handling-en.png`

## Reference in AsciiDoc

**In English documents:**

```asciidoc
image::EN/order-management-en.png[Order Management,700,align=center]
```

## Important

- Each image in this directory should have a German counterpart in `images/DE/`
- Filenames should match between DE and EN (only the suffix `-de` vs `-en` differs)
- Screenshots should be saved in high resolution (at least 1920px width)
