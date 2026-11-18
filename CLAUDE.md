# CEO Reports — instructions for Claude

You prepare analytical reports, Excel workbooks and presentations for the CEO of the plant, using ONLY the read-only reporting API below. Answer in Spanish unless the CEO writes in English.

## The data source

- Base URL: `$REPORT_API_URL`
- Every request: `GET` with header `x-api-key: $REPORT_API_KEY`
- Example: `curl -s -H "x-api-key: $REPORT_API_KEY" "$REPORT_API_URL/reports/catalog"`

**Always start by calling `GET /reports/catalog`.** It lists every report, its parameters, required fields and maximum date range. Do not guess endpoints.

Rules:
- The API is read-only. There is no way to change data, and you must not try (no POST/PUT/DELETE, no SQL).
- Never print the API key in files, slides, chat replies or commits.
- Dates are `YYYY-MM-DD` in plant time (America/Mexico_City).
- A 400 error tells you which parameter is wrong; fix it and retry. A 413 means the range is too big; split it into smaller ranges.
- If a 500 repeats, stop and tell the CEO which report failed and the `requestId`.

## Which report answers what

| Question | Report |
|---|---|
| Line efficiency, produced vs target | `/reports/line-efficiency` |
| Order status, progress %, late orders | `/reports/work-orders` |
| Production for one customer | `/reports/customer-production` |
| Planned load per line / blocked days | `/reports/plan-load` |
| Pending cut requests and close approvals | `/reports/pending-approvals` |
| Cutting: stages, pieces, sizes, markers | `/api/cut-orders/analytics` (+ `/breakdown?id=`) |
| Finished-goods warehouse: received vs assigned vs produced | `/api/finished-warehouse/analytics` (+ `/breakdown?workOrder=`) |
| Merchants: POs created, planning, produced | `/api/merchant/analytics` |
| Mechanics: tickets, bonus, closing time | `/api/mechanics-summary` (if listed in the catalog) |

## Deliverables

- Excel: one sheet per topic, a summary sheet first, real formulas for totals, frozen header rows, dates as dates.
- Presentations: a title slide, one key message per slide, charts built from the report data, a final slide with the data period and source ("LineOps, reporte X, fecha de corte").
- Always state the period covered and the time the data was pulled.
- Never invent or estimate numbers. If a report returns no data for something, say so.
- Save outputs in `reports/YYYY-MM-DD/` in this repo.
