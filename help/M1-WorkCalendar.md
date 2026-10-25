# WorkCalendar help

Publisher: LeonS. Support: atulhritik@hotmail.com (reply within 2 business days).

## Getting started
1. Open a board, click "+" next to the view tabs, choose Apps, then WorkCalendar.
2. Pick a country and a work week (Mon-Fri, Sun-Thu or Mon-Sat) and click Create calendar. This also sets the calendar for the board and picks the date columns.
3. Open the Conflicts tab to see dates on weekends or holidays.

## Calendars
Settings, Calendars. A calendar has a name, a time zone (used only for dates that include a time), a work week (1 to 4 week cycle), holiday packs, closures and extra working days. Turn off a single holiday with its Use switch if your company works that day.
Holiday packs cover 31 countries. Islamic holidays are estimates and may be a day off: turn the row off and add the official day as a closure.

## Automation blocks
In Automate (or the Workflow builder) add a WorkCalendar block.
- Add working days: due date = start date + N working days.
- Set timeline from start + duration: timeline from the first working day on or after the start for N working days.
- Move date off non-working days: moves a date or timeline to the next or previous working day.
- Count working days between two dates: writes the number into a Numbers column.
- Check if a date is a working day: returns true or false, the reason and the next working day.

Definitions: "N working days after" means the N-th working day strictly after the start date. "Duration including start" means a working start date counts as day 1.

## Bulk fix
Conflicts tab: tick rows, choose next or previous working day, click Fix selected. Free fixes up to 10 at a time; paid plans more; the Plus add-on removes the limit. Failed rows stay in the table with monday.com's message.

## Plans
Free, Starter, Pro, Business, and an optional monthly Plus add-on. See the listing for prices and limits. Billing is handled by monday.com. A cancellation within 30 days of the first purchase is refunded automatically by monday.com.

## Data and privacy
WorkCalendar stores settings only (calendars, board settings, editors, a monthly run counter) in monday.com app storage. It does not copy items. Privacy policy: https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/M1-WorkCalendar.txt . To delete your data, email support from your admin address; we delete it within 30 days.

## Holiday data licence
Holiday data (c) date-holidays contributors, CC BY-SA 3.0. The holiday files shipped with the app are shared under the same licence.

## FAQ
- Why does an automation fail with "No calendar set for this board"? Several calendars exist and none is chosen for the board: open the view, Settings, Board settings.
- Why did a date not change on a parent item with subitems? Dates rolled up from subitems cannot be written. Run the automation on subitems.
- Why is "today" different from my calendar's day? The item view uses your browser's date.

## Changelog
1.0.0: first release.
