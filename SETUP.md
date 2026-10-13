# Setup and testing

## Requirements

- A Make account with Microsoft 365 Excel and Gmail connections.
- A Microsoft account with the workbook available through OneDrive.
- An email recipient you control for testing.

## Configure

1. Upload `data/Smart Inventory System.xlsx` to your OneDrive. Keep the Products worksheet and its column order.
2. Create a new Make scenario and import `automation/stock-alert.blueprint.json` using Import blueprint.
3. Reconnect both Microsoft 365 Excel modules to your Microsoft account and reselect your uploaded workbook and Products worksheet.
4. Connect Gmail and replace `your-email@example.com` with your test recipient.
5. In List Worksheet Rows, check the limit (currently 22) is enough for your inventory. Verify the output contains every intended product.
6. Verify the Gmail filter uses F **Value** <= G **Value**, AND J **Text** != today's formatted date.
7. In Update a Worksheet Row, select Values, map Row ID from List Worksheet Rows (`{{6.id}}`), and update J with the same date expression as the filter. Recheck mappings if Make assigns different module IDs during import.
8. Keep column J displayed as `m/d/yyyy`. For a first demonstration, use an earlier date, such as January 1, 2000, for the selected test product's Last Alert Date. Do not reset production alert dates routinely.
9. Save the scenario. Test before enabling the schedule.

## Test

| Test | Expected result |
| --- | --- |
| Stock greater than Reorder Level | No email |
| Stock equal to or lower than Reorder Level, previous alert date | One email and J updated to today |
| Repeat run on the same day | No new email for the same alerted product |
| Inspect the updated row | Other inventory cells and formatting remain intact |

Run the **whole scenario** using Run once, then inspect the email and OneDrive workbook. If Excel update fails, resolve the error before rerunning, as Gmail may already have sent the email.

After the tests pass, set the schedule to **Every 15 minutes**, enable it and save. Make runs the scheduled scenario in the cloud; your laptop need not remain open. Keep the account connections valid and monitor your available Make usage quota.

## Daily use

Edit stock quantities in the connected OneDrive workbook. Leave Last Alert Date to the automation. Update the saved Stock Status and Inventory Value cells separately if quantities change. Keep product IDs unique.

## Publish on GitHub

Extract the ZIP first. Create a repository named `smart-inventory-automation`, then upload the extracted contents, including README.md, SETUP.md and the automation and data folders. Upload the contents rather than just the ZIP so visitors can read the README immediately. Commit the files, then use the repository URL in the Projects section of your CV.

The public blueprint uses placeholder account configuration. Anyone reusing it must connect their own accounts, select their own workbook and recipient, and configure the schedule.
