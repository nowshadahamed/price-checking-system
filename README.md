# Price Checking System

A browser-based price checking and product lookup tool designed for **BMS Company Limited**. It reads product information and prices from multi-sheet Excel workbooks and allows users to search product codes, enter coil quantities, calculate line totals, and generate a grand total instantly.

The system is designed to simplify price verification from large, multi-sheet BMS price-list workbooks without manually searching through individual Excel sheets.

**Live Demo:**
https://nowshadahamed.github.io/price-checking-system/

---

## Features

* **Multi-sheet price lookup**
  Load an Excel workbook and search product codes across all visible sheets.

* **Bulk product-code search**
  Enter or paste multiple product codes at once. Codes can be separated by lines, tabs, commas, or semicolons.

* **Coil quantity calculation**
  Enter the number of coils for each product and automatically calculate:

  `Unit Price × Coil Quantity = Line Total`

* **Grand total calculation**
  Automatically calculates the total value of all selected products.

* **Excel-compatible price handling**
  Prices are extracted from Excel while respecting displayed decimal formatting.

* **Flexible price-cell detection**
  The system can identify prices from cells containing additional characters or text, such as:

  `/9.555`
  `9.555/roll`

* **Hidden data protection**
  Hidden sheets, hidden columns, and hidden rows are excluded from price searches.

* **Japanese and multilingual code matching**
  Unicode normalization is used to improve matching between differently formatted text, including full-width and half-width characters.

* **Column adjustment**
  Users can manually select the Code, Item Name, and Price columns for individual sheets when automatic detection is not suitable.

* **Currency customization**
  The currency symbol can be configured according to the price list.

* **Local processing**
  Excel files are processed directly inside the browser and are not uploaded to an external server.

* **Offline support**
  The application is designed as a standalone HTML application and can operate without a backend server.

---

## Supported File Formats

The system supports:

* `.xlsx`
* `.xls`
* `.csv`

The workbook may contain multiple sheets. The system searches across the visible sheets automatically.

---

## Quick Start

### Use the Live Demo

1. Open the **[Price Checking System](https://nowshadahamed.github.io/price-checking-system/)**.
2. Select your Excel price-list workbook.
3. Enter one or more product codes.
4. Enter the corresponding coil quantities.
5. Click **Show Price**.
6. Review the product name, unit price, quantity, line total, and grand total.

### Example

```text
Product Code:
1104503200
1105010200
1107112211
```

```text
Coil Quantity:
250
20
90
```

The system returns the corresponding products and calculates:

```text
Unit Price × Coils = Line Total
```

---

## Price Calculation

For each product:

```text
Line Total = Unit Price × Coil Quantity
```

The grand total is calculated as:

```text
Grand Total = Sum of All Line Totals
```

### Example

```text
Unit Price: $4.38
Coils: 100

Line Total:
$4.38 × 100 = $438.00
```

---

## How Product Data Is Read

| Data               | Processing Rule                                             |
| ------------------ | ----------------------------------------------------------- |
| **Sheets**         | Visible sheets are searched automatically                   |
| **Hidden Sheets**  | Ignored                                                     |
| **Hidden Rows**    | Ignored                                                     |
| **Hidden Columns** | Ignored                                                     |
| **Code Column**    | Automatically detected from common code-related headers     |
| **Item Name**      | Automatically detected from common product/name headers     |
| **Price Column**   | Automatically detected based on usable numeric price values |
| **Currency**       | Configurable from the Adjust Columns panel                  |
| **Coils**          | Entered by the user                                         |
| **Line Total**     | Unit Price × Coils                                          |
| **Grand Total**    | Sum of displayed line totals                                |

---

## Column Detection

The system attempts to automatically identify the relevant columns.

### Code

Common headers include:

```text
Code
SKU
Barcode
ID
Model
Product Code
```

### Item Name

Common headers include:

```text
Name
Description
Product
Item
Details
```

### Price

The system identifies a usable numeric value from the row while excluding the code and item-name columns.

If automatic detection does not match the workbook structure, the **Adjust Columns** panel can be used to manually select the correct columns.

---

## Bulk Search

Multiple product codes can be entered at once.

Supported separators include:

```text
One code per line
Comma
Tab
Semicolon
```

Example:

```text
1110433310
1104503200
1105010200
1107112211
1101012200
```

The system preserves the entered order when displaying results.

---

## BMS Price List Compatibility

The application is designed to work with BMS multi-sheet price-list workbooks containing product information across sheets such as:

```text
PV
PP
Poly
Manila / Sisal / Abaca
Jute
Vinylon
Vinylon S
Polyester
MS
Tora
Kains
Okada
Binder
Root Wrapping
DCM
and other product-specific sheets
```

This allows users to search a large price workbook without manually opening and checking each sheet.

---

## Handling Repeated Product Codes

If the same product code appears on multiple sheets, the system can display the available matches.

The first matching result is used for the primary calculation, while additional matches are identified separately.

This helps prevent accidental double-counting of the same product.

---

## Price Formatting

The system handles prices with different decimal formats.

Examples:

```text
4.38
23.20
1.430
8.085
12.771
```

The displayed unit price is used when calculating the line total.

For example:

```text
$1.430 × 945 = $1,351.35
```

---

## Privacy

All workbook processing takes place locally in the user's browser.

The uploaded Excel workbook is **not sent to a server** by the application.

This makes the tool suitable for checking internal price-list files without requiring users to upload the workbook to an external service.

> **Important:** Do not commit confidential or proprietary BMS price-list workbooks to a public GitHub repository.

---

## Technology Stack

* **HTML5**
* **CSS3**
* **Vanilla JavaScript**
* **SheetJS Community Edition**
* **Unicode Normalization (NFKC)**
* **GitHub Pages**

The application is implemented as a lightweight browser-based tool without a backend server.

---

## Project Structure

```text
price-checking-system/
│
├── index.html
├── sample-data/
│   └── sample-prices.xlsx
└── README.md
```

---

## Deployment

The project can be hosted using **GitHub Pages**.

After enabling GitHub Pages, the application can be accessed through:

```text
https://nowshadahamed.github.io/price-checking-system/
```

Because the application is client-side, no server-side deployment is required.

---

## Limitations

* The workbook should have identifiable column headers.
* Automatic price detection may require manual adjustment for unusually structured sheets.
* The application is primarily designed for structured BMS-style price-list workbooks.
* Very large Excel files may require more browser memory.
* Product names are displayed according to the source workbook.
* Automatic translation of Japanese product names is not included.
* Internet access is not required after the application and required libraries are available locally.

---

## Use Case

The system is intended to make day-to-day price verification faster and more convenient by replacing repetitive manual Excel searches with a single browser-based interface.

Typical workflow:

```text
Excel Price List
       ↓
Upload Workbook
       ↓
Enter Product Codes
       ↓
Enter Coil Quantities
       ↓
Search All Sheets
       ↓
Retrieve Product & Price
       ↓
Calculate Line Totals
       ↓
Calculate Grand Total
```

---

## Author

**Nowshad Ahamed**
Data Analyst & IT Professional
Chattogram, Bangladesh

* **Website:** https://nowshadahamed.github.io/
* **GitHub:** https://github.com/nowshadahamed
* **LinkedIn:** https://www.linkedin.com/in/nowshad-ahamed/

---

## License

This project is intended for personal and organizational use. Please review the licensing terms of any third-party libraries included in the application before redistribution.
