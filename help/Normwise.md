# Normwise help

Normwise compares the open Revit 2025, 2026 or 2027 project with your office standards file, shows what differs side by side, and copies only the items you choose. A dry run is the default and shows exactly what would change. A real push is one transaction (one undo step) and is checked before it is kept.

> Normwise changes your project when you tick items, turn the dry run off and confirm. Try your first push on a copy of the project. The standards file is opened read-only in the background and is never saved.

The same guide is built into Revit: click the **?** button on the Normwise tab.

## Plans
- Free for 24 hours from the first start (every feature).
- Then a **one-time purchase of $5** unlocks compare and push.
- An optional **add-on at $2.5 per month** unlocks three extras, shown first on the ribbon under **Try add on feature**: the compliance report, the CSV export of the difference and re-check reminders.
- If you stop the add-on, a reminder already stored in the model stays there. Only using the add-on features stops.

## Quick start
1. **Open the project** you want to bring in line with your standards.
2. **Compare.** Normwise tab > Compare and push > Browse > choose the standards file (a Revit project) > Compare. Or pick a project that is already open.
3. **Read the difference.** Select an item to see the standards value and the project value side by side.
4. **Tick and preview.** Tick the items, leave Dry run ticked, click Preview changes. Read the plan and the check result.
5. **Push.** Untick Dry run, click Push to project, confirm. Ctrl+Z undoes a kept push in one step.

## Where to find what
| Panel | Buttons |
|---|---|
| Try add on feature | Compliance report, Export diff (CSV), Re-check reminders (optional subscription) |
| Standards | Compare and push |
| Help | ? (guide, plans and licence, privacy policy, licence terms, data and diagnostics) |

The buttons need an open project (not a family).

## What is compared
- **View templates:** view type, the parameters the template controls and their values, visibility and graphic overrides of categories and subcategories (those that are overridden), and the filters on the template with visibility, enabled state, order and graphic overrides.
- **View filters:** category set and rules (as a set, so the order of rules does not matter).
- **Line patterns, fill patterns, line styles.**
- **Text types and dimension types:** every writable type parameter by name.
- **Browser organisations:** type, sorting and grouping levels (grouping levels need Revit 2026 or later).

Items are matched by name, ignoring letter case and extra spaces. Families, materials, project parameters, sheets and view contents are not compared.

## What a push does
- **Create:** copies an item the project lacks.
- **Update:** changes the existing item in place (line patterns, line styles, text and dimension types, and the categories of a filter).
- **Replace:** view templates and filters that differ in more than categories. The old one is renamed aside, the standards version is copied in under its exact name, the views that used the old one move to the new one, and the old one is removed.
- **Rename:** gives a copy back its proper name when an earlier transfer renamed it by appending a number.
- Items that only the project has are never deleted.

## Name collisions
- A copy named "Name1" (or "Name 1") from an earlier transfer is shown as "Missing (renamed copy exists)". A push renames it back, or replaces it if it also differs.
- When a push copies a view template, Revit may bring along filters it uses. A copy that Revit names with a number appended is merged into the filter the project already had.
- A filter that differs only in its categories is updated in place. If Revit refuses the new categories, the filter is replaced instead and the log says so.

## The check after a push
After the last step the project is read again. Every pushed item must match the standard, no duplicate with a number appended may have appeared, and nothing outside the plan may have changed or disappeared. If anything is wrong you choose between keeping the changes and undoing everything. If a step fails, everything is rolled back automatically.

## Add-on
- **Compliance report:** compare first, then Compliance report. COMPLIANT or NOT COMPLIANT, the share of standards items that match, a table by kind and every item that does not match. Verify only. Save as text or CSV.
- **Export diff (CSV):** one row per property that differs and one per missing or extra item, with the project, standards file and date at the top.
- **Re-check reminders:** every 7, 14, 30, 60 or 90 days, against the standards file you choose. Stored in the model. A short message appears when the project is opened and a re-check is due. It never runs a check by itself.

## Limits
- Not yet run against a wide range of real projects: try a push on a copy first.
- Fill patterns and browser organisations are created when missing but an existing one is not changed.
- A filter that uses a project or shared parameter the target project does not have cannot be copied in full; the check reports it.
- Reading a very large project can take a while.

## Data and privacy
The only thing stored in a model is the re-check reminder (add-on). Settings are in `%AppData%\LeonS\Normwise`, logs in `%LocalAppData%\LeonS\Normwise\logs`. Nothing is sent anywhere except the store licence check. Support: atulhritik@hotmail.com.
