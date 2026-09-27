# PDF Studio

A free, browser-based toolkit for everyday PDF tasks, built on
[pdf-lib](https://pdf-lib.js.org). Everything runs locally in the browser —
files are never uploaded anywhere.

## Features

| Tool                  | What it does                                                                                  |
| --------------------- | --------------------------------------------------------------------------------------------- |
| **Merge PDFs**        | Combine several PDFs into one, in any order                                                   |
| **Organize pages**    | Reorder (drag or Shift + ←/→), rotate, delete, or extract pages by selection or range         |
| **Add text & images** | Click (or use the arrow keys) to place text or an image on any page, with undo                |
| **Watermark**         | Stamp centered, rotated, semi-transparent text on all or some pages                           |
| **Fill a form**       | Fill text fields, checkboxes, radio groups, dropdowns and list boxes; optionally flatten      |
| **Images to PDF**     | Turn JPG / PNG / WebP / GIF images into a PDF, with page size, orientation and margins        |
| **Info & metadata**   | Inspect page count, sizes and dates; edit title, author, subject, keywords, creator, producer |

## Running locally

From the repository root:

```bash
yarn apps:studio
```

This serves `apps/studio/` at http://localhost:8080 and opens it in your
browser. The folder is fully static, so any web server works, e.g.
`npx http-server apps/studio`.

## Deploying

Copy the contents of `apps/studio/` to any static host (GitHub Pages,
Netlify, S3, an nginx folder, …). There is no build step and no server-side
code.

```
apps/studio/
├── index.html
├── styles.css
├── app.js
└── vendor/
    └── pdf-lib.min.js   ← built from this repository
```

## Updating pdf-lib

`vendor/pdf-lib.min.js` is a copy of this repo's UMD build. After changing the
library, rebuild it and refresh the copy:

```bash
yarn build
yarn apps:studio:vendor
```

(`yarn build` currently needs Node 16.14 — newer Node versions break the
`ttypescript` compiler wrapper.)

## Dependencies

- **pdf-lib** (bundled in `vendor/`) does all PDF reading and writing.
- **pdf.js 3.11.174** is loaded from jsDelivr, pinned with a Subresource
  Integrity hash, and is used _only_ to draw page previews. It runs with
  `isEvalSupported: false`, which mitigates CVE-2024-4367. If it can't load
  (e.g. offline), every tool except "Add text & images" still works — you just
  don't get previews.

## Known limitations

These come from pdf-lib itself:

- Password-protected (encrypted) PDFs can't be opened. The app shows a clear
  error instead.
- Text is drawn with the 14 standard PDF fonts, which only support Latin
  characters (WinAnsi). Other scripts show an error rather than a broken PDF.
- Merging several PDFs that contain form fields with the same names can
  produce fields that are linked together.
- There's no text extraction, compression, or rendering to images.

## Browser support

Current versions of Chrome, Edge, Firefox and Safari. The retro 2010-era look
is purely cosmetic — the app uses modern JavaScript (ES2018+).

## Accessibility

- All tools work with the keyboard: drop zones open the file picker with
  Enter/Space, pages in _Organize_ can be selected with Space and moved with
  Shift + ←/→, and content in _Add text & images_ can be positioned with the
  arrow keys (Shift for bigger steps) and added with Enter.
- Ctrl/⌘ + Z undoes the last addition in _Add text & images_.
- Status messages are announced to screen readers, and there's a "Skip to
  content" link.
- The browser warns before you leave the page with unsaved edits.
