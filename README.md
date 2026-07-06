# Enerphy Certificate

A professional training/compliance certificate rendered entirely in LaTeX, inspired by the
classic SGS certificate layout. All graphics (watermark bird, logo mark, accreditation badge,
and the row of flying birds) are drawn with **TikZ**, so the document is fully self-contained —
no external image files are required.

## Files

- `certificate.tex` — layout/engine (rarely edited)
- `company.tex` — **single source of truth for all company data** (offices, contacts, registration, logo, verification, signatory, legal text)
- `course-content/` — swappable per-course syllabus files for page 2
- `certificate.pdf` — compiled output

## Build

Any modern TeX engine works. The simplest is [Tectonic](https://tectonic-typesetting.github.io/):

```bash
tectonic certificate.tex
```

Or with a standard TeX Live install:

```bash
xelatex certificate.tex   # or pdflatex certificate.tex
```

## Customizing

- **Company details** (offices, phones, emails, registration, logo, verification portal,
  signatory, legal fine print) — edit **`company.tex`**. Change it once and it updates
  everywhere on every certificate.
- **Per-certificate details** (recipient, certificate no., course title, scope, validity
  dates) — the **`CONTENT`** block near the top of `certificate.tex`.
- **Page-2 course content** — see `course-content/` (swap the file via `\courseContentFile`).
- **Brand colours** — the **`COLOUR PALETTE`** block in `certificate.tex`.

### Using an image logo
In `company.tex`, set `\coLogoImagetrue` and put the file name in `\coLogoImage`
(e.g. `\def\coLogoImage{enerphy_logo.png}`). The text acronym `\coLogoMark` is used otherwise.
