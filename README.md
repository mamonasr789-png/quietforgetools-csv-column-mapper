# CSV Column Mapper — Offline Schema Mapping Tool

Offline Python CSV tool for **CSV column mapping** / **CSV schema mapping**: rename CSV columns, apply defaults, and write a new file in a chosen target column order. Stdlib only. No network.

**Buy / download (packaged ZIP, £5 GBP):**  
https://quietforgetools.itch.io/csv-column-mapper-offline-schema-mapping-tool?utm_source=github&utm_campaign=csv-column-mapper

> This repository is **documentation and synthetic examples only**. The full packaged tool (`csv_column_mapper.py` + package files) is sold on itch.io. The paid source is not published here.

## Problem

Exports often arrive with one header layout (CRM, shop, accounting) while the next system expects another. Renaming columns by hand is tedious and easy to get wrong. Online converters require uploads.

## What this tool does

- **Rename / map columns** via a small editable JSON file (`target → source`)
- **Ignore** unmapped source columns
- **Preserve** requested target column order
- Optional **defaults / static values** for target columns (including fill-when-blank when both `map` and `defaults` are set)
- **UTF-8** and UTF-8 **with BOM** input; quoted fields; blank cells; Unicode text
- Runs **entirely offline** on your machine (no network, no accounts, no telemetry)

## Synthetic example (before → mapping → after)

**Before** (`examples/source.csv`):

```csv
email_address,given_name,family_name,invoice_total,sku_code,comment
dana@example.com,Dana,Wright,75.25,SKU-100,"Needs invoice PDF, not CSV"
evan@example.com,Evan,Patel,210.00,SKU-200,
faye@example.com,Faye,Ñúñez,15.50,SKU-300,Accented surname OK
```

**Mapping** (`examples/mapping.json`):

```json
{
  "version": 1,
  "target_columns": [
    "Email",
    "First Name",
    "Last Name",
    "Amount",
    "Currency",
    "Comment"
  ],
  "map": {
    "Email": "email_address",
    "First Name": "given_name",
    "Last Name": "family_name",
    "Amount": "invoice_total",
    "Comment": "comment"
  },
  "defaults": {
    "Currency": "GBP"
  }
}
```

**After** (`examples/output.csv` — produced by v1.0.0):

```csv
Email,First Name,Last Name,Amount,Currency,Comment
dana@example.com,Dana,Wright,75.25,GBP,"Needs invoice PDF, not CSV"
evan@example.com,Evan,Patel,210.00,GBP,
faye@example.com,Faye,Ñúñez,15.50,GBP,Accented surname OK
```

`sku_code` is ignored (unmapped). `Currency` is filled from `defaults`.

## Requirements

- Python **3.10+**
- **Standard library only** (`csv`, `json`, `argparse`, `pathlib`) — no pip packages
- Unzip the itch package, then run:

```bash
python3 csv_column_mapper.py \
  --source source.csv \
  --mapping mapping.json \
  --output mapped.csv
```

## What it is not

- Not AI / fuzzy column matching — you write the mapping
- Not relational joins or row merges by key
- Not Excel (`.xlsx`) read/write — CSV in, CSV out
- No guaranteed time savings; results depend on your mapping and source quality
- Not a cloud service — no uploads

## Privacy / offline

Runs entirely on your machine. No network calls, analytics, or phone-home. Your CSVs stay local.

## Version & price

- **Version:** v1.0.0  
- **Price:** £5 GBP  
- **Publisher:** [QuietForgeTools](https://quietforgetools.itch.io/)

## Product page

https://quietforgetools.itch.io/csv-column-mapper-offline-schema-mapping-tool?utm_source=github&utm_campaign=csv-column-mapper

## Examples in this repo

| File | Role |
|---|---|
| `examples/source.csv` | Synthetic source CSV |
| `examples/mapping.json` | Schema map + defaults |
| `examples/output.csv` | Exact v1.0.0 tool output for the above |

All example data is synthetic (`example.com` addresses). No real customer or prospect data.
