# Google Sheets + GitHub Pages Starter

A minimal starter that hosts a static site on **GitHub Pages** and writes/reads data from **Google Sheets** via a tiny Google Apps Script Web App.

## Structure
```
gsheets-pages-starter/
├─ index.html          # Data collection form
├─ display.html        # Read-only table view (pulls JSON from script)
├─ assets/
│  └─ app.js          # Shared JS (helpers)
├─ .nojekyll          # Disable Jekyll on GitHub Pages
└─ server-apps-script/
   ├─ Code.gs         # Apps Script backend (doPost/doGet)
   └─ appsscript.json # Minimal manifest
```

## Quick start

### 1) Create a Google Sheet
- Create a spreadsheet with a sheet named `Leads`
- Header row (row 1): `Timestamp, Name, Email, Phone, City` (you can add more later)

### 2) Deploy the Apps Script backend
1. Open https://script.google.com and create a new project.
2. Paste **server-apps-script/Code.gs** into the editor (replace everything).
3. In `Code.gs`, set:
   - `SHEET_ID` to your spreadsheet ID
   - `SHEET_NAME` to `Leads` (or your sheet name)
   - Optionally set a `SHARED_SECRET` string (and also add it to `assets/app.js` front-end) to prevent drive-by posts.
4. Deploy: **Deploy → New deployment → type: Web app**
   - **Execute as**: Me
   - **Who has access**: Anyone with the link
   - Copy the Web app URL that ends with `/exec`.

### 3) Wire the front-end
- In this folder, open `assets/app.js` and set `const API = "YOUR_WEB_APP_URL";` and (optionally) `const SHARED_SECRET = "..."` to match your backend.
- Edit any form fields you want in `index.html`.
- `display.html` pulls JSON rows to render a table.

### 4) Publish on GitHub Pages
- Create a repo named e.g. `gsheets-pages-starter`
- Push all files; in repo settings, enable **Pages** with the root as source (or `/docs` if you prefer moving files).
- Visit the Pages URL and test submit & view.

## Notes
- **Security**: This uses a simple shared secret (bearer-like) to deter casual abuse. For stronger protection, put the backend behind Cloudflare Workers with a token or set your Apps Script to check `Origin`/`Referer`.
- **CORS**: Apps Script Web Apps work cross-origin for JSON POST/GET. If you ever see a CORS error from your environment, consider a tiny proxy Worker or use `mode: "no-cors"` and handle opaque responses (not recommended).
- **Quota**: Your volume (≈3k rows now + ≈300 rows/year) is well within Apps Script + Sheets free quotas.
- **Extending**: Add fields, add pagination on the client (already included), or add CSV export.

© 2025-09-17
