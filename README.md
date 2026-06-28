# DanteSnap!

Standalone scanner for logging Dante devices on site — Audinate AVIO adapters (QR code), Neutrik and Glensound units (OCR text reading). Same scan-and-photo workflow as MacSnap, separate spreadsheet, separate backend.

This repo is independent of PMY-Scanner (MacSnap). Nothing shared.

## What's in here

- `index.html` — the scanner itself. Single file, no build step. Camera → auto-detects QR (jsQR) or falls back to OCR text reading (Tesseract.js, same preprocessing pipeline proven on MacSnap: grayscale, contrast stretch, 2x upscale) → photo capture → posts both to the Apps Script backend below.
- `Code.gs` — Apps Script backend. Reference copy only; the real one lives in the Apps Script editor attached to your Google Sheet (see setup below).

## Setup

1. **GitHub Pages**: Settings → Pages → deploy from main branch, root. Camera access needs HTTPS, so this step isn't optional — opening `index.html` as a local file will only show the start screen.
2. **Google Sheet**: Create a new Sheet. Extensions → Apps Script → paste in `Code.gs` → Deploy → New deployment → Web app (Execute as: Me, Access: Anyone) → copy the `.../exec` URL.
3. **Wire them together**: open `index.html`, paste that URL into `APPS_SCRIPT_URL` near the top of the `<script>` block, commit.

Sheet columns: LOCATION | DEVICE TYPE | MAC ADDRESS | PICTURE (link).

Device Type fills in automatically for AVIO (detected via QR), or via a quick tap (Neutrik / Glensound) when the scan came from OCR — there's no way to tell those two apart from the label content alone yet. Location is typed in by the installer each scan; the app remembers the last one entered so repeat scans at the same spot are a single tap.

## Known TODO

OCR crop box (in `runOCR()`) is a placeholder guess, not yet tuned to real Neutrik/Glensound label photos. Audinate AVIO doesn't need this — QR scanning reads the full frame, no crop dependency.
