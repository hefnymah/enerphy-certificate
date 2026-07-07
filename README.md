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
  iso-14064-ghg/             # ONE course = ONE folder with ALL its material
    certificate.tex          #   certificate metadata (title, scope, dates, ...)
    content.tex              #   certificate page-2 outline
    participants.csv         #   holders: certno,holder,org (one row = one certificate)
    advert.tex               #   the marketing poster content for this course
  fssc-22000-awareness/      # (another course; advert.tex optional)
    certificate.tex  content.tex  participants.csv
carbon-offsets/
  saf-flight-offset/         # a sustainability certificate (SAF carbon offset)
    certificate.tex  content.tex  participants.csv
```

Everything about a course lives in its own folder — the **certificate** and the
**advert** together. Point `\usecourse{<path>}` (certificate) or the advert's
`\input` at that folder.

Think of it as: `enerphycert.sty` is the class, `src/company.tex` is a config object,
each course folder is a course instance, and each CSV row is a participant instance.

## Public API (used in `main.tex`)

| Command | Purpose |
|---|---|
| `\usecourse{<dir>}` | Load `<dir>/certificate.tex` + its page-2 content |
| `\batchcertificates{<csv>}` | **Mass production** — one certificate per CSV row, all in one PDF |
| `\singlecertificate{<csv>}{<certno>}` | Render just the one row matching that certificate number |
| `\setparticipant{name}{org}{certno}` + `\makecertificate` | Ad-hoc single certificate (no CSV) |

## Two deliverables, one repo

| Build | Entry file | Engine | What it makes |
|---|---|---|---|
| **Certificate** | `main.tex` | `enerphycert.sty` | the fancy 2-page certificate (batch or single) |
| **Course advert** | `main-advert.tex` | `enerphyadvert.sty` | a one-page marketing poster for a course |

Both share `src/company.tex` and `src/assets/`. Advert content lives in the
course's own folder, `courses/<key>/advert.tex` (title, subtitle, intro,
benefits, dates, CTA). To use a real hero photo, drop it in that course folder
(or `src/assets/`) and set
`\def\advHeroImage{yourphoto.jpg}` in the advert file (otherwise a green
gradient hero is drawn).

## Build

```bash
tectonic main.tex          # certificate   (or pdflatex / xelatex)
tectonic main-advert.tex   # course advert
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

- **New participants:** add rows to the folder's `participants.csv`. **Every
  column header becomes a macro** `\<header>` for that row, so the CSV can hold
  *all* per-certificate data — the number, the holder, and any figures. Wrap a
  field containing a comma in `{braces}`; write a literal percent as `\%`.
  - Shared columns used by every certificate: `certno`, `holder`, `org`.
  - The carbon certificate CSV also carries the full footprint figures
    (`grossco`, `saflitres`, `totalco`, `verifyurl`, …) which its `content.tex`
    prints via those `\<header>` macros — nothing is hard-coded.
  - A course can map extra columns by `\renewcommand`-ing `\applyrow`
    (see `carbon-offsets/saf-flight-offset/certificate.tex`).
- **New course:** copy a `courses/<key>/` folder, edit `certificate.tex` +
  `content.tex` + `participants.csv` (+ optional `advert.tex`), and point
  `\usecourse{<new-dir>}` at it in `main.tex`.
- **Company details:** edit `src/company.tex` — updates every certificate everywhere.
- **Left-bar width / colours:** `\barwidth` and the colour palette in `enerphycert.sty`.

## Notes
- Each certificate is 2 pages (certificate + course outline); page numbering resets
  per certificate ("Page 1 of 2", "Page 2 of 2"). Keep each course's `content.tex`
  to one page so the numbering stays aligned.
- The QR encodes a single verification URL (`\verifyQR` in `company.tex`) for all
  certificates; the printed certificate number identifies the holder.
