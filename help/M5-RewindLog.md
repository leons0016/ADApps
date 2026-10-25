# RewindLog help

Publisher: LeonS. Support: atulhritik@hotmail.com (reply within 2 business days).

## Getting started
1. Open a board, click "+" next to the view tabs, choose Apps, then RewindLog.
2. RewindLog reads the last 7 days of the board's activity and lists change sets, newest first.
3. Click "Review & restore" on a change set to see every cell, then restore. Tip: pin the view so you can find it fast after a mistake.

RewindLog is for board owners and admins. Members can look at change sets and diffs; on the Business plan an owner can allow members to restore their own changes. Viewers and guests have no access.

## Change sets explained
A change set groups edits made by the same person, of the same kind (cell values, item renames or group moves), that happened less than N seconds apart (default 30, Settings). A burst of 20 or more cells is closed when slow manual edits follow it, so a mass update is not glued to later single edits. Sets below the minimum cell count (default 5) are hidden until you switch on "Show small changes". The coloured bar shows size: grey under 10 cells, orange 10 to 99, red 100 or more. Edits by automations and integrations appear as "Automation / integration (user id)" when monday cannot name the user.

## The diff screen
For each cell: Before (the value before the earliest selected change), After (the value after the latest selected change) and Now (what the cell holds today).
- Restorable: Now equals After. Selected by default.
- Changed since: someone edited the cell after the change. Not restored unless you choose "Overwrite".
- Already restored: Now equals Before.
- Unsupported: the column type cannot be restored, the status label or dropdown option no longer exists, the column or group was deleted, or the cell is calculated from subitems.
- Item deleted: restore the item from monday's trash first, then refresh.
Rows load as you scroll; the full check runs when you click Restore.

## Restore safely (skip or overwrite)
Before every batch RewindLog reads the cells again and drops any that changed in the meantime (unless you chose Overwrite). Overwrite asks for a second confirmation. The confirm dialog shows the number of cells, the approximate number of monday API calls (monday limits API calls per day, on small plans as low as 1,000), and a reminder that restoring may trigger your board's automations. A progress window shows Restored, Failed and Skipped. Keep the tab open; if you close it, resume from Settings, Restore log, or from the banner on the next visit. Cells that failed keep monday's message and can be retried with "Retry failed". If monday's daily API limit is reached, the run pauses; resume after 00:00 UTC.

## Point in time (Pro)
Timeline tab, "Restore to a point in time": choose date and time (24 hour), the columns, and the scope (whole board, groups, or the items in the board's current filter). Preview lists the cells whose value now differs from the value then. Items created after that time are left out. If monday keeps less history than the time you pick, older changes are not visible and those cells are not listed.

## What can and can't be restored
Can: text, long text, numbers, status, dropdown, date, timeline, people, checkbox, link, email, phone, rating, country, hour, week, tags, item name, and moving an item back to its previous group.
Can't: deleted items, deleted columns, files, updates (comments), subitem structure, connect boards links, formula, mirror, dependency and auto number columns, and cells that monday calculates from subitems. Archived items can be restored if monday accepts the write.

## Activity retention by monday plan
How far back RewindLog can see depends on how long monday.com keeps the activity log for your account, and on your RewindLog plan's range. The header shows the start of the range that was read.

## Undo a restore
Settings, Restore log, "Undo this restore". It puts back the value each cell had just before the restore. Cells that someone changed after the restore are skipped. Undo is always available, also after a downgrade.

## Plans
Free, Starter, Pro, Business, and an optional monthly Plus add-on. See the listing for prices and limits. Billing is handled by monday.com. A cancellation within 30 days of the first purchase is refunded automatically by monday.com.

## Data and privacy
RewindLog reads the activity log in your browser and does not store it. It stores your settings and a record of each restore (item ids, column ids, the written value and the replaced value) in monday.com's app storage, for up to 90 days (365 on Business). Delete them any time in Settings, "Delete all run records". Privacy policy: https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/M5-RewindLog.txt

## Changelog
1.0.0: change sets, diff, batched restore with pause and resume, undo of restores, point-in-time restore, group moves, item names, CSV export of diffs (Plus add-on).
