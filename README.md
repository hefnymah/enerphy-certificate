# Enerphy Certificate

A scalable, package-based LaTeX system for producing Enerphy training/compliance
certificates — a single certificate or a whole course in one batch. Pure LaTeX, so
it runs **locally** (tectonic / pdflatex / xelatex) and on **Overleaf**.

## Project layout (OOP-style separation)

```
main.tex                     # entry point: pick course + production mode, compile
enerphycert.sty              # the "engine" (class): all layout + the public API
src/
  company.tex                # company data — SINGLE SOURCE OF TRUTH
  assets/                    # logos, stamp, signature (referenced by basename)
  backgrounds/               # swappable background art (pick one in main.tex)
    guilloche.tex            #   the banknote engraving (default)
    contours.tex             #   topographic contour rings
    plain.tex                #   no background
courses/
  fssc-22000-awareness/      # a training course (certificate type)
    course.tex               #   metadata (title, scope, dates, ...)
    content.tex              #   page-2 syllabus
    participants.csv         #   holders: certno,name,org  (one row = one certificate)
carbon-offsets/
  saf-flight-offset/         # a sustainability certificate type (SAF carbon offset)
    course.tex               #   metadata (+ its own verification link)
    content.tex              #   page-2 carbon footprint report
    participants.csv         #   holders
```

Each top-level folder (`courses/`, `carbon-offsets/`, …) is a family of
certificate types; each subfolder is one type. Point `\usecourse{<path>}`
at any of them.

Think of it as: `enerphycert.sty` is the class, `src/company.tex` is a config object,
each `courses/<key>/` is a course instance, and each CSV row is a participant instance.

## Public API (used in `main.tex`)

| Command | Purpose |
|---|---|
| `\usecourse{<key>}` | Load `courses/<key>/course.tex` + its page-2 content |
| `\batchcertificates{<csv>}` | **Mass production** — one certificate per CSV row, all in one PDF |
| `\singlecertificate{<csv>}{<certno>}` | Render just the one row matching that certificate number |
| `\setparticipant{name}{org}{certno}` + `\makecertificate` | Ad-hoc single certificate (no CSV) |

## Build

```bash
tectonic main.tex          # or: pdflatex main.tex / xelatex main.tex
```

On **Overleaf**: upload the whole folder, set `main.tex` as the main document, Recompile.

### Choosing the background
In `main.tex`, `\usebackground{guilloche}` — swap for `contours` or `plain`,
or add your own file to `src/backgrounds/` (it just `\renewcommand`s
`\certbackground`).

### Choosing what to produce
Edit the **PRODUCTION MODE** block in `main.tex` and uncomment one line:

- **(A)** `\batchcertificates{...}` — all participants (default).
- **(B)** `\singlecertificate{...}{TRN-2026/0002}` — one person by certificate number.
- **(C)** `\setparticipant{...}\makecertificate` — a one-off, typed in directly.

The output is always `main.pdf`; save/rename per your needs.

## Adding things

- **New participants:** add rows to `courses/<key>/participants.csv`
  (`certno,name,org`; wrap a field containing a comma in `{braces}`).
- **New course:** copy a `courses/<key>/` folder, edit `course.tex` + `content.tex` + `participants.csv`,
  and point `\usecourse{<new-key>}` at it in `main.tex`.
- **Company details:** edit `src/company.tex` — updates every certificate everywhere.
- **Left-bar width / colours:** `\barwidth` and the colour palette in `enerphycert.sty`.

## Notes
- Each certificate is 2 pages (certificate + course outline); page numbering resets
  per certificate ("Page 1 of 2", "Page 2 of 2"). Keep each course's `content.tex`
  to one page so the numbering stays aligned.
- The QR encodes a single verification URL (`\verifyQR` in `company.tex`) for all
  certificates; the printed certificate number identifies the holder.
