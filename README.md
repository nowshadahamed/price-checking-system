# Price Checking System

A browser-based tool that looks up product prices from multi-sheet Excel workbooks. Type or paste up to 100 product codes, add coil counts, and get item names, unit prices, line totals and a grand total in one view.

It runs entirely in the browser as a single HTML file: no server, no installation, and your workbook is never uploaded anywhere.

**Live demo:** (https://nowshadahamed.github.io/price-checking-system/)

<!-- After publishing, add a screenshot:
![Price Checking System screenshot](docs/screenshot.png) -->

---

## Features

- **Search every sheet at once.** Load one workbook (`.xlsx`, `.xls` or `.csv`) and search all of its visible sheets by product code.
- **Bulk lookup.** Paste up to 100 codes from Excel (one per line, or separated by tabs, commas or semicolons). Results keep your order and show which codes were not found.
- **Line totals and grand total.** Enter a coil count for each code in a second box and the tool shows `unit price × coils` for every item and the total of all items.
- **Prices exactly as Excel shows them.** A cell that holds `0.371` but is formatted to show `$0.37` counts as `0.37`; a cell showing `$0.705` counts as `0.705`. Rounding follows Excel (`23.205` shown with two decimals becomes `23.21`).
- **Messy price cells handled.** Cells such as `/9.555` or `9.555/roll` are read as `9.555`; zeros, quantities and other numbers larger than three digits are ignored.
- **Hidden data is ignored.** Hidden sheets, hidden columns, and hidden or filtered-out rows are never counted.
- **Works with mixed-width text.** Half-width and full-width characters (for example Japanese half-width katakana) and Bengali digits are normalised, so codes match however they were typed.
- **Fully offline.** The Excel reader is bundled inside the page, so it works without an internet connection.

## Quick start

**Use the hosted page**

1. Open the live demo link above.
2. Choose your Excel workbook. The first row of every sheet must contain column names.
3. Enter a product code in the first box, and optionally a coil count in the second box. For many codes, paste one code per line in the first box and the matching coil counts, in the same order, in the second box.
4. Press **Show price**.

## Try it with the sample data

`sample-data/sample-prices.xlsx` is a small fictional workbook that demonstrates the rules below (it contains a hidden column, a hidden row and a hidden sheet). Paste these codes and coil counts:

| Code   | Coils | What the sample shows                                    | Result                    |
| ------ | ----: | -------------------------------------------------------- | ------------------------- |
| R-1001 |   100 | Normal price                                             | 4.38 × 100 = 438.00       |
| R-1002 |   100 | Cell holds 0.371 but is formatted to show `$0.37`        | 0.37 × 100 = 37.00        |
| R-1003 |    50 | Cell formatted to three decimals                         | 0.705 × 50 = 35.25        |
| R-1004 |    10 | Price is 0, so it is not treated as a price              | Price not found           |
| R-1999 |    10 | Row is hidden                                            | Not found                 |
| T-2001 |    20 | Price stored as text (`/9.555`)                          | 9.555 × 20 = 191.10       |
| T-2002 |    10 | 23.205 with a two-decimal format                         | 23.21 × 10 = 232.10       |
| T-2003 |    25 | Three-decimal price                                      | 7.125 × 25 = 178.13       |
| A-9001 |     5 | Sheet is hidden                                          | Not found                 |

Grand total for these entries: **$1,111.58** (305 coils in total).

## How the data is read

| Topic | Rule |
| --- | --- |
| Sheets | All visible sheets are searched. Hidden sheets, hidden columns and hidden or filtered-out rows are skipped. |
| Header row | The first non-empty row of each sheet holds the column names. |
| Code column | Detected from the header (`code`, `sku`, `barcode`, `id`, `model`), otherwise the first column. |
| Name column | Detected from the header (`name`, `description`, `product`, `item`, `details`), otherwise the first text column. |
| Price | By default, the last cell in the row that contains a usable number (code and name columns excluded). Text around the number is dropped, and a number right after a `/` is preferred. A price must be above 0 and below 1000, and dates are ignored. |
| Displayed decimals | Follows what Excel shows, with at least 2 and at most 3 decimals. |
| Totals | Line total = displayed unit price × coils, worked out to 2 decimals. The grand total is the sum of the line totals shown. |
| Repeated codes | A code that exists on several sheets lists every match, but only the first match counts toward the grand total (the others are flagged). |
| Not found in the code column | The other columns are searched too. For a single code, similar codes are suggested. |
| Overrides | The **Adjust columns** panel lets you choose the code, name and price columns for each sheet and set the currency symbol (default `$`). |
| Memory | The last workbook is remembered in your browser's local storage, so you do not have to reload it each time. Very large workbooks may exceed the browser's storage limit and need to be loaded again. |

## Privacy

All processing happens in your browser. The workbook is read locally and is never sent to a server. Do not commit real price lists to a public repository; keep the workbook on your own machine and load it through the page.

## Tech stack

- HTML, CSS and vanilla JavaScript in a single file
- [SheetJS Community Edition](https://sheetjs.com/) (Apache-2.0) for reading Excel files, bundled inline
- Unicode normalisation (NFKC) for consistent code matching

```
.
├── index.html            # the whole application (works offline)
├── sample-data/
│   └── sample-prices.xlsx
└── README.md
```

## Limitations

- The first row of each sheet must contain column names.
- The automatic price rule assumes the price is the last numeric cell in a row. If a sheet is laid out differently, pick the price column manually under **Adjust columns**.
- At most 100 codes are searched at a time.
- Item names are shown as they appear in the workbook. Automatic English translation of names is not available on static hosting.

## Author

Nowshad, Data Analyst
