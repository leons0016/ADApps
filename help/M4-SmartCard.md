# SmartCard - Help

SmartCard turns the monday.com item card into a smart form with conditional fields: fields appear, hide or become required based on other values, with a different layout for each team.

**SmartCard hides fields in its own card only. It is not a security feature.** The data is still visible on the board to anyone with board access. See the FAQ.

Publisher: LeonS. Support: atulhritik@hotmail.com (we reply within 2 business days).

## Getting started
1. Open an item, click "+" on the item card, choose Apps, then SmartCard.
2. Board owners and account admins see "Build a smarter item card". Choose **Start from my columns** (one section "Details" with every column in board order) or a template: Sales lead, Support ticket, Employee onboarding, Call disposition. Templates match columns by type and title; slots without a match are left empty for you to pick.
3. The layout builder opens. Everyone who opens an item on the board then sees the card.
Members see the message "This board has no SmartCard layout yet. Ask a board owner." until a layout exists.

## The item card
- The header shows the item name (click to edit) and a label: "Complete" or "N required fields missing". Click the label to jump to the first missing field.
- Each section can be collapsed. SmartCard remembers this for you, per board.
- Each field saves when you change it: pickers immediately, text fields when you leave the field or press Enter. A small "Saving" and "Saved" shows next to the label.
- If monday.com rejects a change (a validation rule or a permission), the field goes back to its old value and monday.com's message appears under the field.
- The card re-reads the item every 30 seconds while it is visible and when you return to the tab, so changes made by other people appear. The section logic itself needs no API call, so a section appears right after you change the value.
- Column types the card cannot edit (formula, mirror, files, dependency and others) are shown as read-only text.
- The footer says how many fields are hidden by the layout and offers "Show all fields" (a read-only list).
- View-only users and guests see the card with editing off.

## Layout builder
Open it from the item menu "Edit layout" (owners, admins and listed layout editors).
- Left: sections and fields. Use the arrow buttons to reorder. "+ Section" and "+ Field" (only columns not yet used in this layout; "Item name" can be added once).
- Right top: live preview with the current item's values. You can change values in the preview, but nothing is saved.
- Right bottom: properties. Section: title (1 to 60 characters), help text (up to 300), collapsed by default, "Show this section when". Field: label (up to 60), help text (up to 300), width full or half, read-only, visibility (Always, Show when, Hide when), required (Never, Always, When), and with the Plus add-on a default value.
- Save checks: every section has a title; every rule is finished; a field can't depend on itself; a column that controls a section must be outside it (unless it is also shown in an always-visible section); the layout name is unique and 1 to 40 characters; the configuration of a board stays under 1 MB.
- If two owners edit the same board at the same time, the second save is refused with a message and nothing is overwritten. Reload and apply your change again.

## Conditions and operators
Rules compare a column (or the Group, or the Current user) with a value. Combine rules with "All of (AND)" or "Any of (OR)". One level of nested groups is allowed.

| Column type | Operators |
|---|---|
| Status | is any of, is none of, is empty, is not empty |
| Dropdown | contains any of, contains all of, contains none of, is empty, is not empty |
| Numbers, Rating | =, not equal, <, <=, >, >=, is empty, is not empty |
| Date | is empty, is not empty, is before, is after, is on, is within next N days, is overdue |
| Timeline | start or end is empty, start or end is before or after |
| Text, Long text, Email, Phone, Link | is empty, is not empty, contains, does not contain, equals |
| Checkbox | is checked, is not checked |
| People | includes me, includes any of, is empty, is not empty |
| Group | is any of, is none of |
| Current user | is in team any of, is any of |
| Formula, Mirror | contains, equals, is empty (values from monday.com may lag) |

Empty values: "is none of" and "not equal" are **true when the field is empty** (the way people read "status is not Done"). Other comparisons on an empty value are false.
Dates are compared as calendar dates in your browser's time zone (the automation block uses the UTC date).
Status and dropdown rules store the label's id, so renaming a label does not break a rule. If a column or label is deleted the rule never matches and the builder marks it.
Hiding a field never clears its value. A field inside a hidden section stays hidden even when its own condition is true.

## Required fields
A field is required when its "Required" setting says so and it is visible, or when monday.com's own validation requires the column. A required field that is empty counts as missing.

## Team layouts (Starter and higher)
A board can have several layouts. In the builder tab "Who sees this layout" assign a layout to teams and/or people. The first match wins: layouts that list the user, then layouts that list one of the user's teams in the order of the match list, then the default layout, which is for everyone else. Owners can use "Preview as..." from the item menu; a preview never saves. If a user's teams cannot be read, the default layout is used.

## Work with monday validation
SmartCard reads monday.com's data validation for the board (needs a Pro or Enterprise monday.com plan and API version 2026-07 or later).
- Required columns are marked required in the card, and if the layout hides one that is empty, a section "Required by monday" shows it at the top.
- Required-if rules (if column X has these values, column Y is required) are evaluated in the card. Other kinds of rules, such as value limits, are listed in the builder tab "monday validation" and still enforced by monday.com when a value is saved.
- If the validation cannot be read, the card works without it and owners see "Couldn't read monday validation rules" in the builder.
For hard enforcement set required fields in monday.com's data validation. SmartCard only guides data entry and cannot block saves made on the board, through the API or by automations.

## Work queue (Pro and higher)
Add the board view "SmartCard queue".
- Choose a queue: **All items**, **Board filter** (the items of the board view's current filter) or a preset made by an owner.
- The list shows the item name, the main status, a badge with the number of missing required fields and the initials of the first people column of the layout. Search by name. The footer shows the number of items.
- Click an item to work on it in the form. **Save & next** (or Ctrl+Enter, Cmd+Enter on Mac) goes to the next item. If required fields are missing, a message tells you, and you still move on. Fields are saved as you change them.
- Presets (menu "Edit queues", owners): a name, a filter built with the same rules as conditions, a sort column and direction, and "Only items with missing required fields". Pro has up to 5 presets, Business is unlimited.
- Filters are evaluated in your browser over the items loaded with the board API. Status, People and Date rules in a top-level AND filter are also sent to monday.com to download fewer items. On boards with more than 5,000 items a preset needs at least one such rule: otherwise the queue says "This queue is too broad for a board this size. Add a Status, People or Date rule." The queue loads at most 10,000 items.

## Plus add-on
Available on any paid plan as a "plus" plan.
- **Check SmartCard rules** automation block: inputs board, item and optionally a layout name (the default layout when empty). Outputs `isComplete`, `missingCount` and `missingFields`. Use it for recipes such as "When status changes to Done, if not complete, set status back and notify". It uses the default layout, the item's values and monday.com's required columns; date rules use the UTC date and "includes me" and team rules use the user that monday.com reports in the block call.
- **Layout import and export**: copy a layout to another board as JSON (assignments are not exported).
- **Default values**: a field can get a value (a fixed text or number, or a date such as today plus 2 days) when it appears and is empty. It is written only when a field changes from hidden to visible during your session, never on first load, and never for read-only fields.

## FAQ
**Can I hide data from people?** No. SmartCard only changes what its card shows. The data is still visible on the board to anyone with access. Use monday.com board permissions to restrict access.
**Why does a field show "Required at this stage"?** Its rule or monday.com's validation says it is needed now. Fill it and the header turns green.
**Does hiding a field delete its value?** No. Values are never cleared.
**A rule seems ignored.** On the Free plan only the first 3 rules of a layout work. Open the builder to see the banner.
**Does SmartCard work on subitems?** It depends on whether monday.com shows app item views on subitems. Subitems of classic boards live on their own board, which has its own layout.
**Where is my data stored?** Layouts and queue presets are in monday.com's app storage. Item data is read to show the card and is not stored by the app.

## Plans
| | Free | Starter (3 USD) | Pro (5 USD) | Business (7 USD) |
|---|---|---|---|---|
| Boards with SmartCard | 1 | 5 | unlimited | unlimited |
| Layouts per board | 1 | 2 | 5 | unlimited |
| Layouts for teams or people | no | yes | yes | yes |
| Rules per layout | 3 | unlimited | unlimited | unlimited |
| Work queue presets | no queue | no queue | 5 | unlimited |
Sections, fields and monday validation awareness are available on every plan. Plus add-on: adds 1.5 / 2.5 / 3.5 USD per month to Starter / Pro / Business. Prices are per month in USD and billed by monday.com. Paid plans start with a trial whose length is set by monday.com. When a plan ends, layouts are kept; layouts above the limit are locked, only the default layout is used, and rules above the limit are ignored (the card then shows more, never less).

## Privacy
SmartCard stores layouts, rules and queue presets only. See the privacy policy: https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/M4-SmartCard.txt

## Changelog
### 1.0.0
First release: item card with conditional fields, layout builder, team layouts, monday validation awareness, work queue, Plus add-on (rules-check block, import and export, default values).
