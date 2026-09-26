# Setup guide

## 1. Deploy the updated Code.gs

Replace your current `Code.gs` with the one I gave you (adds
`bulkAddBlockProduction` and `bulkAddColumnProgress`, needed by the Import
tabs — everything else is unchanged from what you already had, including
your own fix for the GET/payload write-action issue).

1. Open your Sheet → **Extensions → Apps Script**.
2. Select all of `Code.gs`, delete it, paste in the new version.
3. Save (disk icon).
4. **Deploy → Manage deployments → Edit (pencil) → Version: New version → Deploy.**
5. Test: log in on the main app, add a test expense.

## 2. Create the new Sheet (fresh data), keep the old one as archive

1. Open your current Google Sheet → **File → Make a copy**. The original is
   now untouched — your archive of everything so far.
2. In the **copy**, clear the data rows (not headers) of: `Expenses`,
   `BlockProduction`, `ColumnProgress`, `CashBook`. Keep `Users` and `Sites`
   as they are, so logins carry over unchanged.
3. In the copy's **Extensions → Apps Script**, paste in the updated `Code.gs`
   from Step 1, plus `Categories.gs` (your existing category/guessCategory
   file — copy it over unchanged).
4. **Deploy → New deployment → Web app** → Execute as: Me, Who has access:
   Anyone. Copy the new `/exec` URL.

## 3. Point every app at the new URL

Update the `API_URL` constant in each of these to the new deployment URL:
- `js/api.js` (main app)
- `block-production-app/index.html`
- `excavation-concrete-app/index.html`
- `cash-book-app/index.html`

The old Sheet and its deployment URL keep working untouched if you ever need
to look something up in the archive.

## 4. Import your historical data

`BSU_MB2_converted_to_Benue_format.xlsx` (from report-9, treated as the
complete record) is ready to check and import:

1. Open it and set the **Site** column to the correct site name on every
   sheet (it currently defaults to "MB2").
2. Spot-check the Column Base Cubic/Bags split and the category assignment
   per the notes on the Summary sheet.
3. Split its rows into the block-production-app and excavation-concrete-app
   templates (Column Base + Block Setting → Block Production's Production
   sheet doesn't apply directly — Column Base/Trenches belong in the
   Excavation & Concrete app; Block Setting belongs in Block Production).
4. Upload through each app's **Import** tab, review the flagged rows, then
   import.

`CashBook-5_ready_to_import.xlsx` is already in the cash-book-app's exact
template format (268 lines, order preserved, dates forward-filled) — no
conversion needed, just:

1. Open it and set **Site** on each row if it shouldn't default to "ALL".
2. Upload through the Cash Book app's **Import** tab. Since the running
   balance depends on row order, fix any rows flagged as errors before
   importing rather than deleting them — the Import button stays disabled
   until there are none.

## 5. Remove the pages from the main app (once feeders are confirmed working)

In `js/layout.js`, remove the sidebar links for Block Production,
Excavation of Trenches, and Cash Book, since those now live in their own
apps and write to the same Sheet as the main app.

## What's still open

- Project Health / Final Report as read-only views in the main app
- Plain-text passwords in the Users sheet (worth fixing at some point,
  unrelated to this rollout)
