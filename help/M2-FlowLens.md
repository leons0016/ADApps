# FlowLens help

Publisher: LeonS. Support: atulhritik@hotmail.com (reply within 2 business days).

## Getting started
1. Open a board, click "+" next to the view tabs, choose Apps, then FlowLens.
2. Step 1: pick the status column (FlowLens suggests the one titled "Status").
3. Step 2: check the stage of each status: Backlog / To do, In progress, Done or Ignored. Use the arrows to order the In progress statuses.
4. Step 3: FlowLens reads the board's activity log. You can cancel; the next open continues where it stopped.
Only board owners and account admins can set up or change the workflow.

## Stage mapping
FlowLens pre-maps by name: statuses marked as done by monday or named Done, Closed, Shipped, Deployed, Complete, Resolved or Released become Done; Stuck, Blocked, Working on it, In progress, Review, QA and Testing become In progress; Not started, To do, Backlog and the empty status become Backlog / To do. Labels you add later appear as "Unmapped" in Settings and count as Ignored until you map them. Changing the mapping recalculates every chart at once without reading the log again.

## How we measure
- **Start**: the first time the item enters any In progress status.
- **Done**: the first time it enters a Done status after Start (or the archive / group rule in Settings, Business plan).
- **Cycle time** = Done minus Start, in calendar days with one decimal.
- **Lead time** = Done minus the time the item was created. If the creation time is not known, the first change FlowLens saw is used and the value is marked "at least".
- **Time in status S** = the sum of all intervals the item spent in S. Only items that left S inside the range are counted, unless you switch on "Count current time for items still in a status".
- **Throughput** = number of items whose Done falls in the period. Items that went straight to Done count here and in lead time, but have no cycle time ("Skipped start").
- **WIP on a day** = items in an In progress status at the end of that day. **Age** = now minus Start, for items currently in progress.
- **Percentiles** use the nearest-rank method: p85 is a real value from your data, "85% of items took this long or less".
- **Reopen rule** (Settings): if a Done item moves back, either keep counting from the first start (default) or start a new cycle (both cycles are counted).
- Deleted items are left out of every chart. Day boundaries use your browser's time zone.

## Tabs
Overview, Time in status, Cycle time, Throughput, CFD, Aging WIP, Forecast, Settings. CFD and Aging WIP need Starter or above, Forecast needs Business.

## Forecasts explained
Pick "When will N items be done?" or "How many items by a date?", choose how many weeks of throughput to use (4, 8 or 12) and click Run forecast. FlowLens draws 10,000 random days from your own daily throughput of that period. "85%" means 85 of 100 simulations finished by that date (or reached at least that many items). It needs at least 10 finished items in the chosen weeks. The same click always gives the same answer; "Run again" uses a new random seed. A forecast is a likelihood, not a promise.

## History keeper and retention
monday keeps the activity log for a limited time that depends on your monday plan. On the Pro and Business plans FlowLens saves its own compact copy of the status changes it reads (item numbers, times, status numbers, user numbers, never item names), and adds new changes each time anyone opens the view (at most once every 5 minutes per board). The label "History since" shows the oldest change FlowLens has. On Free and Starter nothing is saved: FlowLens reads the activity log each time. Settings has "Rebuild history" and "Delete saved history".

## Filters and export
Range, groups, person (if the board has a People column), item name and subitems (multi-level boards) apply to every tab and are remembered per user. With the Plus add-on the download button saves the current tab's table as CSV (UTF-8, opens in Excel). If your browser blocks the download inside monday, the CSV is copied to the clipboard.

## Item view and dashboard widget
The item view "Status timeline" lists every status of the item with the date, who changed it and the time spent. It reads the activity log directly, so it can start later than the item if monday has already dropped older entries. The "Flow summary" widget shows, for each connected board, cycle time p85, throughput per week, WIP against limits and aging items for the last 90 days. Without the Plus add-on it shows the first connected board.

## Plans
Free, Starter, Pro, Business and an optional monthly Plus add-on. See the listing for prices. Billing is handled by monday.com. A cancellation within 30 days of the first purchase is refunded automatically by monday.com.

## Data and privacy
FlowLens is read-only and never changes your boards. Settings and saved history live in monday.com app storage; nothing is sent elsewhere. Privacy policy: https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/M2-FlowLens.txt . To delete everything, email support from your admin address; we delete it within 30 days.

## FAQ
- **Why does my history start on a certain date?** monday keeps the activity log for a limited time, depending on your monday plan. FlowLens can only save changes it has seen. Pro and Business keep what they read from then on.
- **Why does an item not appear?** FlowLens only shows items you can see yourself. Archived items are shown to account admins only.
- **Why is my cycle time empty?** Cycle time needs an item that entered an In progress status and then a Done status. Check the mapping in Settings.
- **Why does the number differ from my own calculation?** Compare the definitions above, especially the reopen rule and calendar days.
- **Status labels were renamed or reordered.** FlowLens matches history by label text first and number second and warns in Settings; older history may show old names.

## Changelog
1.0.0: first release.
