# Course content (page 2)

Each `.tex` file here is a **swappable syllabus** printed as page 2 of the
certificate. The certificate stays the same; only this content changes per course.

## Switch course
In `certificate.tex` (CONTENT block), point to the file you want:

```latex
\newcommand{\courseContentFile}{course-content/fssc-22000-awareness}
```

## Turn the second page off
```latex
\coursepagefalse     % certificate becomes a single page
```

## Add a new course
1. Copy `fssc-22000-awareness.tex` to e.g. `iso-9001-lead-auditor.tex`.
2. Edit the content.
3. Set `\courseContentFile` to `course-content/iso-9001-lead-auditor`.

## Helpers available in content files
- `\coursesection{Title}` — heading with an accent underline
- Colours: `ink`, `softink`, `accent`
- Standard LaTeX (`itemize`, `tabular`, `\textbf`, …). Long content flows
  onto further pages, which stay fully branded with correct `Page X of N`.
