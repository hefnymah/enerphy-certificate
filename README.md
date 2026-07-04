# Enerphy Certificate

A professional training/compliance certificate rendered entirely in LaTeX, inspired by the
classic SGS certificate layout. All graphics (watermark bird, logo mark, accreditation badge,
and the row of flying birds) are drawn with **TikZ**, so the document is fully self-contained —
no external image files are required.

## Files

- `certificate.tex` — LaTeX source
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

All editable text lives in the **`CONTENT`** block near the top of `certificate.tex`:
recipient name, course/standard title, scope, validity dates, signatory, badge text and the
fine print. The **`COLOUR PALETTE`** block controls the brand colours (including the bird row).

To use a real logo image instead of the drawn text mark, add `\usepackage{graphicx}` and
replace `\logoMark` / `\providerMark` with
`\includegraphics[height=1.6cm]{yourlogo.png}`.
