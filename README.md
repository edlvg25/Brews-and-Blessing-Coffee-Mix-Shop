# Brews & Blessings — POS & Inventory Frontend

A static HTML/CSS/JavaScript frontend prototype based on the supplied sign-in reference, mockup collections, Context-Level DFD, and Project Charter.

## Run
1. Extract this folder.
2. Open the folder in VS Code.
3. Start it with VS Code Live Server (recommended).
4. Open `index.html`.
5. On the first launch, create the first Owner / Manager account. Then sign in using that username and password.

## Account and login behavior
- A default Owner / Manager account is seeded automatically on first launch:
  - Display name: Franchesca Jannel Camacho
  - Username: chesca@gmail.com
  - Password: jannel123
- There is no public create-account or first-owner setup option on the sign-in page.
- Only a signed-in Owner / Manager can create additional Owner/Manager and Barista accounts from **Settings & Backup → Add user account**.
- Account creation asks for the person's display name, username, role, password, and password confirmation.
- Barista accounts cannot open Owner pages; attempts to open an Owner workspace redirect to the Barista dashboard.
- Login checks the saved username and password. Incorrect credentials do not grant access and show an error.
- The display name and role of the currently logged-in account appear in the top bar.
- The app blocks access to a workspace that does not match the logged-in account's role.

## Empty starting data
- Orders, inventory, menu products, expenses, refund requests, shift checks and audit log start empty.
- Dashboard totals and report calculations start at ₱0.00 / 0 and update as you add records.
- No demo menu items, stock quantities, sales or expenses are preloaded.
- Data is saved in the browser's localStorage. On the first launch of this updated version, the old demo data is cleared once. Your subsequently entered data is retained in that browser.

## Screens
- `index.html` — staff sign-in and first Owner setup
- `owner/dashboard.html` — owner overview
- `owner/pos.html` — order creation, cash payment, order status, cancellation
- `owner/inventory.html` — ingredient inventory and stock updates
- `owner/costing.html` — drinks menu and product cost
- `owner/reports.html` — sales/expense/profit summary and CSV export
- `owner/approvals.html` — refund approval, daily sales verification, void entry
- `owner/shift.html` — opening/closing shift check
- `owner/settings.html` — user accounts, roles, backup and restore
- `barista/dashboard.html` — barista overview
- `barista/pos.html` — POS, cash payments, order status, cancellation and refund request
- `barista/stock.html` — read-only stock status
- `barista/shift.html` — shift check submission

## Scope alignment
- Exactly two operational roles: Owner/Manager and POS User/Employee (Barista).
- Barista inventory access is read-only.
- Cash-only payments.
- Refunds require Owner/Manager approval and are limited to orders not yet served.
- Cancellations require a reason; voids are Owner/Manager-only and require a reason.
- Drinks only: coffee, non-coffee and refreshers. Food items are excluded.
- No customer login, customer-facing ordering app, online payments, loyalty/CRM, payroll, or multi-branch management.

## Important prototype limitation
This is a frontend-only demo. Data and accounts are stored in browser localStorage and are not shared between devices. The seeded Owner credentials and other passwords are stored in browser localStorage and are not securely protected in a frontend-only prototype. Before real deployment, connect the forms to your Node/Express backend, validate credentials server-side, hash passwords, enforce role permissions on the server, and store records in MySQL. Stock deductions/restoration based on ingredient recipes, audit logging, refund cash hand-over, and true external backup storage also require backend integration.


## Dynamic account list
- When an Owner creates an account in Settings, the account is saved and the visible account table and account count update immediately without refreshing the page.
- Invalid or duplicate account details keep the account modal open so the Owner can correct them.
- Newly created accounts are available for sign-in immediately in the same browser.

- The Owner can delete other accounts from the live account table. The primary Owner account, the currently signed-in account, and the last remaining Owner account are protected to prevent accidental lockout. Deletions update the table immediately without a refresh.

## Starter Menu and Ingredient Inventory

The starter drinks menu follows the Coffee, Non-Coffee, and Refreshers product list in the Project Charter's Company Products table. The starter ingredient inventory is a suggested setup list based on those drink categories; the charter does not specify exact recipe ingredients or opening quantities. Inventory quantities start at zero and must be entered by the Owner/Manager using the shop's actual counts. Example selling prices and product costs are placeholders and should be replaced with approved business values.

Owner/Manager users can add, edit, and delete drinks under **Menu & Costing**. Deleting a drink removes it from future POS selection while preserving its details in existing order records.

## Automatic Shift Counts and Recipe-Based Inventory

- Barista shift checks automatically calculate the system cash balance from recorded cash sales. Cancelled, voided, and refunded orders are excluded.
- Shift inventory counts are generated from the current system ingredient quantities; they are not a manual physical stocktake.
- POS orders deduct the ingredients assigned to each drink recipe, including packaging items where configured.
- Drinks become unavailable in the POS when any required ingredient is missing or insufficient.
- Cancelling, voiding, or approving a refund for an order restores the ingredients previously deducted for that order.
- The recipe quantities in `script.js` are suggested starting estimates because the project charter does not specify exact recipes or per-cup ingredient quantities. The Owner/developer must adjust `recipeForProduct()` to match the shop's approved recipes before operational use.
- Ingredient quantities must be stocked in the Owner's Inventory module before drinks become available in the POS.
