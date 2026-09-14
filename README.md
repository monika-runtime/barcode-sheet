# Barcode Sheet Generator

Paste a list of values, get a printable sheet of barcodes. Runs entirely client-side —
no server, no uploads, no tracking. Deployed at <https://barcode.shmingus.net>.

## What it does

- Paste one value per line (blank lines ignored, duplicates kept)
- Pick a symbology — the format applies to every value on the sheet
- Live preview of the exact page that prints
- Print straight to a label sheet or plain paper, with cut guides

## Getting around

The form is tabbed — **Values**, **Barcode**, **Sheet**, **Advanced** — so only the
section you are working in is on screen and nothing has to be scrolled into view. Each
tab's card shows a one-line summary of the settings that live in it, and the **Advanced**
tab carries a count of anything you have changed from its defaults, so a set-once option
can never be left changed and forgotten.

## Adding dates

**Date** inserts today's date as `YYYYMMDD` — compact, sortable, and safe for every
symbology. The caret beside it opens the rest of the formats, each shown with today's
value as a preview: `YYYY-MM-DD`, `YYMMDD`, `MMDDYYYY`, `DDMMYYYY`, `DD/MM/YYYY`,
`YYYYMMDD-HHMM`, `YYYYMMDDHHMMSS`, `YYYY-MM-DD_HH-MM-SS`, `YYDDD` (Julian day of year),
`YYYY-Www` (ISO week) and `MMM-DD-YYYY`.

Dates come from the device's local date, not UTC. Digit-only symbologies (ITF, MSI,
pharmacode, EAN, UPC) cannot encode a dashed or slashed date — the insert still happens
and the status bar flags it under **Skipped** rather than printing a broken code.

## Symbologies

| Format | Constraint |
|---|---|
| Code 128 (auto A/B/C) | anything — default, general purpose |
| Code 128 B / C | text / numeric pairs only |
| Code 39, Code 93 | alphanumeric |
| ITF (Interlaced 2 of 5) | digits, **even count** |
| MSI | digits |
| Pharmacode | 3–131070 |
| Codabar | digits + `-$:/.+` |
| EAN-13 | 12 or 13 digits (check digit auto) |
| EAN-8 | 7 or 8 digits |
| UPC-A | 11 or 12 digits |
| ITF-14 | 13 or 14 digits |

Values that don't fit the chosen format are skipped and listed under **Skipped** in the
status bar instead of silently producing a broken barcode.

## The bar-width readout

The status bar shows the narrowest bar module on the sheet in millimetres. Below
~0.19 mm (7.5 mil) the bars stop being reproducible on a 300 dpi printer, so the
readout turns red and tells you to widen the label or shorten the values. This matters
because a barcode that renders fine on screen can still smear into an unreadable blob
on paper.

## Layout controls

Sheet geometry is in millimetres: page size (Letter / A4 / A5 / custom), page margin,
columns, label width/height, gap between labels, and inner padding. Enabling
**Auto-fit columns** derives the column count from the page width and label width.

Two small labels per row and a wide page is usually the fastest route to a dense sheet —
`45 mm × 18 mm`, 4 columns fits 44 labels on Letter.

## Repositioning labels

Drag any label in the preview to move it; it snaps to the label grid. Dropping onto an
occupied cell swaps the two, and dropping onto blank paper leaves a deliberately empty
cell — which is what you want when printing onto a partly-used label sheet. Focus a label
and use the arrow keys to make the same move without a mouse.

The layout is remembered alongside the other settings. The status bar shows
`Layout: custom` once the arrangement differs from paste order, and **Reset layout**
returns everything to list order. Editing the values list rebuilds the layout in paste
order, so a stale arrangement can never be printed by accident.

## Printing notes

- Print at 100% / "actual size" — **not** "fit to page", or the label pitch drifts and
  labels won't line up with die-cut stock.
- Turn off any ink-saving / toner-saver mode; thin bars are the first thing it erodes.
- Silence the print dialog's headers/footers (in Chrome: uncheck "Headers and footers")
  so URLs don't print across the top of your sheet.
- `Print the number under each barcode` is the human-readable line. It is not part of the
  encoding — turn it off to leave more vertical room for taller bars.

## Files

- `index.html` — the whole app (HTML, CSS, JS inline)
- `jsbarcode.min.js` — [JsBarcode](https://github.com/lindell/JsBarcode) 3.12.1, MIT

## Local use

Just open `index.html` in a browser. No build step, no dependencies to install.

## Hosting note

Netlify's edge injects its own HUD/badge overlay (`/.netlify/scripts/hud`) into published
sites. The site's `hud_enabled` flag is already `false` and the overlay is injected
anyway, so the page hides it in CSS and removes the injected nodes as they appear — see
the final script in `index.html`. It never loads on the local file.

## License

MIT for the app; JsBarcode is MIT, © Johan Lindell.
