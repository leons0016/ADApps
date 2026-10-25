# SubTables help

Publisher: LeonS. Support: atulhritik@hotmail.com (reply within 2 business days).

## Getting started
1. Open any item, click "+" in the item card, choose Apps, then SubTables. (Item views are added per board and then appear on every item of that board.)
2. Click "Create a sub-table", name it (for example Expenses), pick a preset or add your own columns, and click Create.
3. Type a name in "Add row" and press Enter. Click any cell to edit it.

## How SubTables stores data
Every sub-table is a normal monday board. Its rows are normal items, linked to the parent item through a Connect boards column. SubTables shows and edits them inside the item card and keeps only its settings (and your template rows) in monday app storage. Because rows are real items, they are searchable, can be used in automations, dashboards and exports, and are never deleted when you uninstall the app. We suggest putting the sub-table boards in a folder such as "SubTables data".

## The Create sub-table wizard
Name (1 to 40 characters), columns (up to 25; presets Blank, Deliverables, Expenses, Sessions, Contacts, Checklist), then where to put the board (workspace, optional folder, board kind). It creates the board, the columns, a hidden technical "Order" Numbers column and a two-way Connect boards column on your board. If a step fails you see which one; "Retry remaining steps" never creates anything twice.

## Using existing connected boards
Configure sub-tables, "Use an existing board": choose a Connect boards column of the board, which linked board to show, the columns, and optionally an Order column (or "Add order column").

## Supported column types
Editable inline: text, long text, numbers, status, dropdown, date, people, checkbox, link, email, phone, rating, timeline. Other types (for example files, formula, mirror) are shown read-only; use "Open" to edit them in monday. Status and dropdown cells offer only labels that already exist; SubTables never creates labels.

## Rows
- Add: type a name and press Enter. Empty names are blocked.
- Edit: click a cell; Enter or Tab saves, Esc cancels. A red border with a tooltip shows why a value was refused.
- Copy duplicates a row. Remove archives the item in monday (it can be restored from the board).
- Reorder: drag the handle, or use the arrow buttons. Reordering needs an Order column. Sorting by a column header only changes how you see the table, not the saved order.
- Search filters by row name. Narrow item views show cards instead of a table.

## Rollups
Configure sub-tables, tab Rollups. Functions: Sum, Average, Min, Max, Count, Count where (a status label), Earliest date, Latest date, Concatenate names. Each rollup writes into a column of the parent board (Numbers for numbers, Date for dates, Text for names). "Create target column" makes the column for you. Rollups are recomputed after every add, edit, duplicate, remove and template insert made in the item view, and only changed values are written. A column that monday calculates (formula, mirror, rollup of subitems) cannot be written; SubTables tells you instead of pretending. Do not fill a rollup column by hand: the next rollup overwrites it.
Edits made directly on the sub-table's board do not recalculate on their own. With the add-on, use the "Recalculate parent rollups" automation block (it needs a two-way connection).

## Templates
Configure sub-tables, tab Templates: create a template with named rows and values. In the item view, "Insert template" adds all its rows at once. Values for columns that were deleted are skipped with a notice.

## Add-on features (the "plus" plans)
- SubTables grid: a board view listing all items (first 500), each expandable to its sub-tables.
- Paste rows: paste from Excel or Sheets. The first pasted column is the row name; the next columns fill the sub-table's editable columns in order. A first line with column titles is detected. Dates use YYYY-MM-DD. Up to 200 rows per paste.
- Automation blocks: see the blocks guide (docs/blocks.md in the app) for fields.

## Plans
Free, Starter, Pro, Business, and an optional monthly add-on. See the listing for prices and limits. Billing is handled by monday.com. A cancellation within 30 days of the first purchase is refunded automatically by monday.com. When a plan ends or is downgraded, nothing is deleted: extra sub-tables, rows and boards become read-only, extra rollups pause, extra templates stay stored.

## Permissions
monday board permissions decide everything. If you cannot see the child board you see a "no access" message. If monday refuses a write, the cell reverts and the buttons are hidden for the rest of your session. Only account admins and board owners can configure sub-tables.

## Two-way connections
With a two-way connection each new row points at its parent, which avoids lost links when several people add rows at the same time. The wizard always creates a two-way connection. With a one-way connection SubTables reads the parent's list, writes the new list and checks it; if it cannot confirm after 3 tries the row shows "Not linked. Retry" instead of losing the link silently.

## Data and privacy
SubTables stores settings and template rows only, in monday.com app storage. Privacy policy: https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/M3-SubTables.txt . To delete your settings, email support from your admin address; we delete them within 30 days. Your rows are your monday items and stay.

## FAQ
- Why can't I edit a row? You may be over your plan's row limit (rows beyond it are read-only), have view-only access, or monday refused the change on the child board.
- Why does a row say "Shared with 1 other item"? A duplicated parent item can keep the same links, so two parents share the row. SubTables only tells you.
- A linked item is "on another board". The connect column links several boards; one sub-table shows one board.
- Why does the rollup column show an old value after I edited on the child board? Rollups update from the item view. Use the Recalculate block (add-on) or open the item.

## Changelog
1.0.0: first release.
