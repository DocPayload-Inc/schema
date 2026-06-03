# DocPayload

**Plain JSON. Every document.**

DocPayload is a declarative, JSON-based document definition format. Describe your document once — layout, styling, content, security — and DocPayload renders it to **PDF, DOCX, or SVG**, pixel-perfect and production-ready.

---

## Why DocPayload

Generating documents shouldn't mean wrestling with low-level PDF libraries, fragile HTML-to-PDF pipelines, or proprietary template editors. DocPayload gives you a single, IDE-friendly schema that covers everything from a one-page invoice to a signed, encrypted, multi-section contract.

- **Declarative** — describe *what* the document should look like, not *how* to draw it.
- **Schema-first** — point your IDE at the published schema and get autocomplete, validation, and inline docs for free.
- **Composable** — build reusable components once, reference them across documents.
- **Production-grade** — encryption, digital signatures (PAdES), watermarks, and bookmarks built in.

---

## Quick Start

Reference the official schema in your JSON for instant IDE validation:

```json
{
  "$schema": "https://docpayload.com/docs/schemas/v1/document-definition.schema.json",
  "document": {
    "pageSetup": {
      "size": "LETTER",
      "orientation": "portrait",
      "margins": [40, 40, 40, 40]
    },
    "metadata": {
      "title": "Invoice #1042",
      "author": "Acme Corp"
    },
    "content": [
      { "h1": "Invoice #1042" },
      { "p": "Thank you for your business." }
    ]
  }
}
```

That's a valid DocPayload document. Send it to the DocPayload API and you get a PDF back.

### Try a real document

Walk through one of these end-to-end tutorials to see DocPayload applied to a real-world document:

- [Void cheque](https://docpayload.com/docs/tutorials/void-check) — banking primitives, MICR line, precise field placement
- [Certificate suite](https://docpayload.com/docs/tutorials/certificate-suite) — reusable certificate templates with shared styling
- [Verdania VRT-4 tax form](https://docpayload.com/docs/tutorials/verdania-tax-vrt4) — government-style form layout with structured fields
- [Refund workflow](https://docpayload.com/docs/tutorials/refund-workflow) — multi-step business document with data binding
- [Lab result report](https://docpayload.com/docs/tutorials/lab-result-report) — tabular clinical data with headers, footers, and metadata
- [Lease agreement](https://docpayload.com/docs/tutorials/lease-agreement) — long-form contract with sections, signatures, and bookmarks
- [Multilingual device guide](https://docpayload.com/docs/tutorials/multilingual-device-guide) — multi-script content (CJK, Devanagari, Thai, Latin) with custom fonts

---

## What You Can Build

DocPayload is designed to cover the full range of business and consumer document workflows:

| Capability             | What it gives you                                                           |
| ---------------------- | --------------------------------------------------------------------------- |
| **Page layout**        | LETTER, LEGAL, A0–A10, B1–B10, custom sizes, portrait/landscape, margins    |
| **Rich content**       | Headings, paragraphs, lists, tables, images, multi-column layouts           |
| **Styling**            | 200+ CSS-like properties — typography, flexbox, borders, transforms         |
| **Headers & footers**  | Per-page or per-section, with separators                                    |
| **Tables**             | Rowspan, colspan, fixed/auto widths, collapse, repeating headers            |
| **Barcodes**           | 63 formats — QR, Code 128, Data Matrix, PDF417, EAN-13, UPC-A, and more     |
| **Vector shapes**      | SVG-like canvas: lines, polygons, arcs, paths, text-on-path                 |
| **Watermarks**         | Single or repeating, visible across pages                                   |
| **Components**         | Define once, reuse across documents via `use` references                    |
| **Bookmarks & TOC**    | Auto-generated outlines with configurable tab leaders                       |
| **Shortcodes**         | Inline `[bracket]` tags for formatting, links, form fields, media, barcodes |
| **Internationalization** | 25+ languages, CJK / Indic / Arabic scripts, mixed LTR + RTL in one doc   |
| **Diagnostics**        | Inline or appendix-style validation reports (info / warning / error levels) |

---

## Shortcodes

Shortcodes are inline `[bracket]` tags you drop into any text element — paragraphs, headings (`h1`–`h5`), table cells, list items — to apply formatting, embed media, insert form fields, or link to other content. No wrapper container required, and tags nest freely.

**Syntax:** `[tag]content[/tag]` or `[tag, param1, param2]content[/tag]`

```json
{
  "p": "[b][color, #CC0000]URGENT:[/color][/b] Review [i]before[/i] [u]end of day[/u]."
}
```

### Available shortcodes

| Category         | Shortcodes                                                                                                |
| ---------------- | --------------------------------------------------------------------------------------------------------- |
| **Typography**   | `[bold]`/`[b]`, `[italic]`/`[i]`, `[bolditalic]`/`[bi]`, `[underline]`/`[u]`, `[strike]`/`[s]`, `[caps]`, `[font, Name]` |
| **HTML aliases** | `[strong]`, `[em]`, `[cite]`, `[ins]`, `[del]`, `[mark]`                                                  |
| **Text props**   | `[fontsize, N]`/`[size, N]`, `[color, c]`, `[letterspacing, N]`/`[ls, N]`, `[highlight, c]`/`[hl, c]`, `[align, l\|r\|c\|j]`, `[superscript]`/`[sup]`, `[subscript]`/`[sub]` |
| **Layout**       | `[linebreak]`/`[br]`, `[tab, X, Y]`                                                                       |
| **Media**        | `[image, path, W\|H]`/`[img, path, W\|H]`, `[barcode, type, code, W\|H]`                                  |
| **Form fields**  | `[textfield]`, `[textarea]`, `[checkbox]`, `[radio]`, `[choicefield]`, `[listbox]`                        |
| **Links**        | `[link, URL]…[/link]`, `[goto, dest]…[/goto]` (internal jumps)                                            |

Shortcodes are inline-only and combine naturally with the `style` system, so you get the best of both worlds: structured styling for blocks, lightweight inline tagging for the words inside them.

**More:** [Shortcodes overview](https://docpayload.com/docs/shortcodes/overview)

---

## Data Binding

Build templates once, render them with any payload. Data binding lets you reference values from an external JSON object using dot-separated paths prefixed with `$data` — no template engine, no expression language, just clean placeholders that resolve at generation time.

### Reference syntax

| Pattern                              | Example                                   |
| ------------------------------------ | ----------------------------------------- |
| Top-level property                   | `$data.name`                              |
| Nested object                        | `$data.client.companyName`                |
| Deeply nested                        | `$data.client.address.city`               |
| Array indexing                       | `$data.items[0].name`                     |
| Chained access                       | `$data.company.bills[3].amount`           |

### Where references resolve

References work inside paragraph text, headings, table cells, list items, form-field defaults, headers, footers, columns, and **inside shortcodes** — so you can mix them freely:

```json
{ "p": "[b]Client:[/b] $data.client.companyName — [color, #CC0000]$data.client.address.city[/color]" }
```

### Passing data

Send the data alongside the template at generation time:

```json
{
  "client":  { "companyName": "GlobalTech Industries" },
  "user":    { "name": "Laura Bennett", "email": "laura.bennett@shipforge.io" },
  "invoice": { "number": "INV-2026-0317", "totalAmount": "$24,500" }
}
```

### Iterating over arrays

For repeating content (invoice line items, list entries, table rows), point a content block at an array using `source`:

```json
{ "source": "$data.items" }
```

DocPayload walks the array and renders the template against each item.

### Globals

Built-in `$global.*` tokens are also available for things like the current date, time, and page numbers — no payload needed.

### Resolution rules

- **Missing references are preserved as literal text** — e.g. `$data.missing.key` renders verbatim, making typos easy to spot during template authoring.
- **Out-of-bounds array indices** are likewise preserved rather than failing the render.

**More:** [Data binding overview](https://docpayload.com/docs/data-binding/overview)

---

## Internationalization

DocPayload is built for documents that cross borders and scripts:

- **25+ languages** out of the box — Latin, CJK (Chinese, Japanese, Korean), Indic (Devanagari), Thai, Arabic, Hebrew, and more.
- **Complex script shaping** — Devanagari conjuncts, Thai stacked vowels and tone marks, and paragraph-length CJK all render correctly.
- **RTL support** — set `baseDirection: "rtl"` at the paragraph level. Mix LTR and RTL blocks in the same document without redesigning your layout.
- **Bilingual layouts** — side-by-side parallel columns (e.g. English + Arabic), with each block reading in its native direction.
- **Mixed directionality in tables** — keep line items LTR while showing parallel LTR/RTL totals so either side can audit.
- **Noto font family** — CJK ships as a single ~16 MB Noto Sans CJK family covering Chinese, Japanese, and Korean. Custom fonts (e.g. Noto Sans Arabic) can be loaded per document.
- **On-demand subsetting** — only the glyphs you actually use are embedded, keeping output files in the hundreds of kilobytes even for multi-script documents.
- **Multi-script composition** — combine scripts in a single element (e.g. a canvas seal with concentric `textPath` rings carrying Latin + CJK outer and Devanagari + Thai inner).

**Tutorials:**
- [Multilingual device guide](https://docpayload.com/docs/tutorials/multilingual-device-guide)
- [Bilingual invoice](https://docpayload.com/docs/tutorials/bilingual-invoice)

---

## Security & Compliance

For documents that need to hold up in regulated environments:

- **Encryption** — RC4 (40/128-bit) and AES (128/256-bit)
- **Digital signatures** — PAdES and Detached signing
- **Certification levels** — `NO_CHANGES_PERMITTED`, `FORM_FIELDS_MODIFICATION`, `ANNOTATION_MODIFICATION`
- **Field-level locking** — restrict form filling and annotation per signature field
- **Selective encryption** — encrypt embedded files only, exclude metadata, etc.

---

## Schema Reference

The full, authoritative schema is published at:

**https://docpayload.com/docs/schemas/v1/document-definition.schema.json**

### Root structure

A DocPayload payload contains exactly one of:

- `document` — a complete document definition
- `component` — a reusable component definition
- `documentDefinition` — alternative root key for documents

### Core building blocks

```
DocumentDefinition
├── pageSetup           — size, orientation, margins, borders, background
├── metadata            — title, author, subject, keywords, version
├── header / footer     — repeating page chrome with separators
├── content[]           — main document body
├── styles {}           — named, reusable style definitions
├── fonts {}            — custom font family registration
├── images {}           — pre-defined image resources
├── use[]               — references to reusable components
├── watermarks[]        — visible watermarks per page
├── encryption          — PDF security settings
├── signature           — digital signature spec (PAdES / Detached)
├── attachment          — embedded file attachments
├── bookmark            — document outline / navigation
└── diagnostics         — validation and logging configuration
```

### Content elements

The `content` array accepts a flexible mix of:

- **Text** — `p`, `h1`–`h5`, table cells (`th`, `td`)
- **Lists** — ordered (`ol`) and unordered (`ul`)
- **Tables** — rows, columns, widths, spans, repeating headers
- **Images** — with sizing and positioning
- **Shapes** — SVG-like canvas with lines, rectangles, circles, ellipses, polygons, paths, arcs, named shapes (triangle, diamond, pentagon, hexagon, octagon, arrows, chevrons), text, and text-on-path
- **Barcodes** — 63 supported formats
- **Separators** — solid, dashed, dotted, double, groove, inset, outset, ridge, and more
- **Breaks** — page, section, line
- **Multi-column sections**

### Styling

Every styleable element accepts a CSS-like `style` object covering:

- **Typography** — `fontSize`, `fontFamily`, `fontWeight` (100–900, normal, bold, bolder, lighter), `fontStyle` (normal, italic, oblique), `lineHeight`, `letterSpacing`, `wordSpacing`
- **Layout** — `width`, `height`, `margin*`, `padding*`, flexbox (`flex*`, `justify*`, `align*`)
- **Color** — `color`, `backgroundColor`, `fill`, `stroke`, `opacity`
- **Borders** — `border*`, `borderRadius*`, `borderCollapse`
- **Tables** — `tableLayout`, `borderCollapse` (`separate` | `collapse`)
- **Text** — `textAlign` (left | center | right | justify), `verticalAlign` (top | middle | bottom | baseline)
- **Transforms** — `transform`, `rotate`, `skew`
- **Columns** — `columnCount`
- **Direction** — `baseDirection` (`ltr` | `rtl`)

**Full reference:** [Styles documentation](https://docpayload.com/docs/document-definition/styles)

---

## IDE Integration

Add the `$schema` field at the top of any DocPayload JSON file and your editor (VS Code, JetBrains, Neovim, etc.) will give you:

- Autocomplete for every property
- Inline documentation
- Real-time validation
- Enum-value suggestions

```json
{
  "$schema": "https://docpayload.com/docs/schemas/v1/document-definition.schema.json"
}
```

---

## Versioning

This README documents the **v1** schema. The schema follows JSON Schema Draft-07. Breaking changes will be published under a new version path (`/v2/`, `/v3/`, …) — v1 will remain stable and available.

---

## Learn More

- **Schema (canonical)** — https://docpayload.com/docs/schemas/v1/document-definition.schema.json
- **Documentation** — https://docpayload.com/docs
- **Website** — https://docpayload.com

---

*DocPayload — plain JSON. Every document.*
