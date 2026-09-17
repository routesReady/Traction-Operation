# Traction Operation — West Central Railway, Kota Division

## Included in this deployment-ready build
- Existing Dashboard, Saved Data / Export and Data Sources workflows preserved.
- New **Summary** page with From Date, To Date and DIR (UP/DN) filters.
- Summary calculations run from **SavedRow MongoDB data**, not the reference `summary.xlsx`.
- Date-wise summary with clickable train counts that open Saved Data / Export with Date + DIR pre-filtered.
- Summary Excel export with the exact requested summary columns.
- `Shed_Remark` and `OEM` fields on the Dashboard; both are saved and remain editable during Modify → Update.
- `Shed_Remark` and `OEM` included in Saved Data / Export and Excel export. Existing records remain valid and display blank values until updated.
- Dynamic dropdown extraction from Excel **Data Validation** metadata. The application no longer expects the dropdown choices to exist as cell values.
- The source workbook's original bytes remain stored, so downloading the uploaded workbook preserves its Excel validation lists.
- Mobile/tablet responsive fixes: page-level horizontal overflow is prevented, header/grid sizing is constrained, controls stack correctly, and wide data tables scroll inside their own containers.
- Existing modify workflow remains an upsert on `(searchDate, sourceRow)` and does not intentionally create duplicate records.

## Deployment
### Render backend
- Root Directory: `server`
- Build Command: `npm install`
- Start Command: `npm start`
- Environment variables:
  - `MONGODB_URI`
  - `JWT_SECRET`
  - `UPLOAD_PASSWORD`
  - `CLIENT_ORIGIN=https://traction-operation.vercel.app`

The CORS configuration also accepts the Traction Operation Vercel preview-domain pattern.

### Vercel frontend
- Root Directory: `client`
- Build Command: `npm run build`
- Output Directory: `dist`
- Environment variable:
  - `VITE_API_URL=https://YOUR-RENDER-SERVICE.onrender.com`

The frontend appends `/api` automatically when the supplied URL does not already end in `/api`.

## Important data-safety behavior
- No collection is dropped.
- No existing SavedRow is deleted by these changes.
- `summary.xlsx` is never imported into MongoDB and is not used for summary calculations.
- Old SavedRow documents remain compatible because the new fields are optional/default to blank.
- Re-uploading `TRAINS_PER_DAY.xlsx` refreshes the stored dropdown metadata from its Excel Data Validation rules.

## Local development
### Backend
```bash
cd server
npm install
copy .env.example .env
npm run dev
```

### Frontend
```bash
cd client
npm install
npm run dev
```

Local `.env` example:
```env
CLIENT_ORIGIN=http://localhost:5173
```

Local Vite environment:
```env
VITE_API_URL=http://localhost:5000
```
