# Smart Inventory Automation

An Excel-based inventory monitoring prototype built by **Mohammad Madi**, using Microsoft 365 Excel, OneDrive, Make, and Gmail.

The project helps a small shop identify products that need restocking. It checks inventory on a 15-minute schedule and sends an email for each eligible product. After a successful email, it records the alert date to suppress further alerts for that product on the same Qatar calendar day.

## Implemented features

- Product inventory with stock quantities and individual reorder thresholds.
- Alerts when **Stock <= Reorder Level**, including equality.
- HTML emails with product name, ID, current stock and reorder level.
- A Last Alert Date field to prevent repeated daily notifications during normal successful execution.
- Scheduled checks every 15 minutes in the original Make scenario.
- Stock Status and Inventory Value columns in the supplied workbook.

## Workflow

1. Microsoft 365 Excel **List Worksheet Rows** reads the Products worksheet.
2. A filter checks Stock Value against Reorder Level Value and compares Last Alert Date Text with today's date.
3. Gmail **Send an email** sends the product alert.
4. Microsoft 365 Excel **Update a Worksheet Row** uses the original Row ID to update column J after Gmail succeeds.

Date expression used by both the filter and update:

```text
{{formatDate(now; "M/D/YYYY"; "Asia/Qatar")}}
```

One alert means **one email per product per calendar day**, rather than one combined email for all products. A product that remains below its threshold can receive another alert on the following day. Inventory quantities currently need to be maintained manually.

## Files

| File | Purpose |
| --- | --- |
| `data/Smart Inventory System.xlsx` | Supplied workbook containing 20 sample products and saved alert dates |
| `automation/stock-alert.blueprint.json` | Make blueprint with personal connection references removed and a placeholder recipient |
| `SETUP.md` | Setup, operating instructions and test procedure |

In the supplied snapshot, **7 of 20 products** satisfy Stock <= Reorder Level. Column J retains historical dates from the original demonstration. No new live email test was performed while preparing this repository.

## Scope and limitations

- This is a no-code inventory alert prototype. POS integration, barcode-driven stock updates and online-store synchronization are future extensions.
- A barcode scanner reads an identifier; a POS integration must provide completed sales and quantity changes before stock can update automatically.
- The blueprint currently reads a maximum of **22 rows**. Increase that setting and check the returned row count when adding products.
- The blueprint does not replace your account connections or scheduling configuration. Reconnect apps and set the schedule after importing.
- The date comparison depends on column J displaying `m/d/yyyy`. Keep that display format or adjust both sides of the comparison consistently.
- If an email succeeds but the following Excel update fails, the next execution can repeat that alert. This prototype does not provide a transactional delivery guarantee.
- The imported workbook's status and inventory-value cells are saved values, not formulas. Update them when editing quantities; the alert filter uses columns F and G directly.

## Future development

- Connect a POS system's sales events to inventory updates.
- Synchronize stock with an e-commerce platform.
- Add a combined daily summary and stronger delivery-state handling.

## Portfolio description

Built an inventory monitoring prototype using Excel, OneDrive, Make and Gmail, with scheduled stock checks, product-specific reorder thresholds and daily alert tracking.
Testing and planned use

Self-testing — my embroidery project

I tested the system with products and supplies from my own embroidery project. I entered their quantities in Excel and set a reorder level for each item. When I tested a low-stock item, the system sent an email and updated its Last Alert Date. I also checked that running it again on the same day didn’t send another alert for that item.

Stock quantities still need to be updated manually, so the alerts depend on keeping the spreadsheet up to date.

Planned trial — a friend’s family shop

I plan to try the system in my friend’s father’s shop to see how useful it is in daily work. The first trial would use manual stock updates in Excel. Connecting it to the shop’s cashier system would need more work, depending on whether its software can export sales data or connect to other tools.

This trial has not started yet.