# Texas school finance data

Texas Education Agency PEIMS financial data for the state, the 20 ESC regions, every district
and every campus, packaged so the numbers live in one place instead of TEA's report server.

The data lives in this repo's **Releases**, not in git:

- `tea_actual_<level>.parquet` (and `_wide`): TEA's official actual financial reports as tidy rows.
  `level` is state, region, district or campus; `org_code` is `STATE`, the 2-digit region, the
  6-digit county-district number or the 9-digit campus number; `district` carries the 6-digit
  number on district and campus rows. Region and campus files are added as they are collected.
- `peims_ledger_actual.parquet`, `peims_ledger_budget.parquet`: the PEIMS line-item ledger
  (fund x function x object x program intent), 1991-92 onward.
- `f33_*`, `nces_*`: Census and NCES F-33 school finance, keyed on the NCES LEAID, with the
  `nces_tea_crosswalk` to TEA's county-district number.
- `raw_*.tar.gz`: the TEA report pages and PEIMS downloads as fetched, the source of record.
- `manifest.csv`: size, SHA-256 and row count of every file.

Campus reports carry expenditures only, and campus totals do not add up to the district's:
central and districtwide spending stays at the district.

Every file joins on county-district number (or campus number) and school year (`end_year`), the
same keys as the STAAR and TELPAS results in
[texas-assessment-data](https://github.com/mrworthington/texas-assessment-data).
Built and validated by `R/22`-`R/23` in `statewide-peims-explorer`.

Read an asset directly, e.g. with DuckDB:
`SELECT * FROM 'https://github.com/mrworthington/texas-school-finance-data/releases/download/<tag>/tea_actual_district.parquet'`
