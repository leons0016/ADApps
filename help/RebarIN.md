# RebarIN help

RebarIN adds IS 2502 rebar shapes and Indian bar bending schedules (BBS) to Revit 2025, 2026 and 2027. It reads the rebar in your model and builds the schedule: shape codes, cutting lengths with bend deductions and hook allowances, laps for long bars, unit weights, member-wise grouping and a summary by diameter. Publisher: LeonS. Support: atulhritik@hotmail.com.

> RebarIN is a scheduling tool, not a design tool. The default rule values are common Indian practice and must be checked against IS 2502, IS 456, SP 34, IS 1786 and your project specification. The engineer of record must check quantities and lengths before use.

The same guide is built into Revit: click the **?** button on the RebarIN tab.

## Plans
- Free for 24 hours from the first start (every feature).
- Then a **one-time purchase of $7** unlocks everything except the add-on.
- An optional **add-on at $3.5 per month** unlocks three extras, shown first on the ribbon under **Try add on feature**: Export XLSX, Firm rules (with rule-set import and export) and Bar-mark renumbering schemes.
- If you stop the add-on, your schedules, parameters and rule sets stay. Only those three features stop.

## Quick start
1. **Load the shapes.** RebarIN tab > Load shapes > Load. Shapes already in the project are skipped. Bars modelled with Revit's own shapes need no new shape: RebarIN reads their geometry.
2. **Open the BBS.** Click BBS, choose the scope (whole model is the default), the grouping and the rule set, then Calculate. The first time, RebarIN asks whether to add its RIN_ parameters to the rebar. Choose Add so that tags and schedules work.
3. **Put it on a view.** Click Create / update drafting view. The schedule is drawn in a drafting view named "BBS - Whole model". Place that view on a sheet. When the model changes, click Refresh BBS.

## Where to find what
| Panel | Buttons |
|---|---|
| Try add on feature | Export XLSX, Firm rules, Bar marks (optional subscription) |
| Shapes | Load shapes, Map shapes, Shape catalogue |
| Schedule | BBS, Refresh BBS, Native schedule |
| Settings | Rules |
| Help | ? (guide, plans and licence, privacy policy, licence terms, data and diagnostics) |

The buttons need an open project (not a family).

## Load shapes
Creates the IS 2502 shapes as Revit rebar shapes named like `IS T8-1 Rect stirrup`. Each shape created by RebarIN carries its code inside, so renaming it does not break the mapping. The list shows a sketch, code, name, dimension letters, IS 2502 table reference and status (New, Exists, Exists - in use, Needs template). Failed shapes are listed with Revit's reason; the others still load.

Some shapes cannot be built reliably in code: angle shapes (T6-1, T6-2), the column splice bar (T7-2), the hairpin (S-02) and the spiral (T9-2). They are copied from a template project when one is installed; otherwise they are listed as "Needs template" and you can draw them in Revit's shape editor with the same name pattern. Bars of those shapes are still scheduled, from their geometry.

## Map shapes and the RIN_ parameters
Every bar gets a shape code in this order: a RebarIN shape gives its code and reads its dimensions from the bar; any other bar is read from its geometry (out-to-out dimensions); a bar that matches nothing is listed as **FF** (free form) with Revit's centreline length. Varying-length bars are listed as FF, grouped by length.

RebarIN adds shared parameters to Structural Rebar: `RIN_ShapeCode`, `RIN_BarMark`, `RIN_Member`, `RIN_CuttingLength`, `RIN_TotalLength`, `RIN_UnitWeight`, `RIN_TotalWeight`, `RIN_Remarks`, `RIN_MarkManual` and `RIN_A` to `RIN_E`.
- `RIN_Member` and `RIN_Remarks` are yours: RebarIN never overwrites them, and a typed member replaces the host mark.
- `RIN_BarMark` is written by a whole-model BBS. A typed mark is flagged in `RIN_MarkManual` and kept.
- Bars owned by another user (workshared models) are listed as "Not updated (owned by ...)" and are still scheduled. Bars in a Revit group are scheduled but not written.
- An updater recalculates the parameters of bars you add or change and marks BBS views out of date. It does not renumber marks.

## The BBS window
- **Scope:** whole model, selected elements, current view, a level, or a host category (Beams, Columns, Slabs, Walls, Foundations, Stairs).
- **Group by:** host category then member, member (host mark), or level then member. **Rule set:** project rules, your rule files, firm files (add-on). **Show:** detailed rows or summary only.
- Group by, Show and Wastage re-arrange the same bars without reading the model again; Calculate reads it.
- **Grid columns:** Sl. No., Member, Bar mark, Shape code, Shape sketch with dims, Dia, Spacing, No. of members, Bars per member, Total bars, Cutting length, Total length, Unit weight, Total weight, Remarks. Double-click a member header to fold it. Click a row to select its bars. Edit Member and Remarks in the grid; typing a Bar mark is part of the Bar marks add-on feature.
- **Right side:** summary by diameter (length, unit weight, kg, tonnes), grand total, wastage % (0 to 15) and warnings. Click a warning to select its bars.
- The schedule is always metric (mm, m, kg), also in imperial projects.

Members with the same name on several hosts merge into "No. of members" when their bars are identical; otherwise they split into groups such as "B1 (x3)" and "B1 (L5)".

## How lengths are calculated
- **Allowance method (default):** sum of the shape's dimensions measured out to out, minus bend deductions, plus hook allowances, by the shape's formula (see Shape catalogue), rounded (default: nearest 5 mm). The other method uses Revit's centreline length.
- **Check:** if the allowance length differs from the model's centreline length by more than 5% or 50 mm, a warning names the bars.
- **Laps:** if a bar is longer than the stock length (default 12 m) and laps are on, the lap length is the larger of Ld and the minimum tension lap, rounded up to the next 10 mm. The steel grade comes from the bar type's material name (for example Fe500) and the concrete grade from the host's structural material (for example M25). Missing grades fall back to Fe500 and the default concrete grade, with a warning.
- **Weights:** unit weight from the table of nominal masses in the rule set (or d x d / 162); total weight = total length x unit weight, in kg and tonnes.

## Drafting views and Refresh BBS
- The view is named "BBS - " and the scope, at scale 1:1, drawn with Revit lines and text: header block (company, project, rule set, date), column headers, a header row per member, rows with shape sketches, the summary by diameter and the engineering note.
- Long schedules continue on views named like "BBS - Whole model (2/3)". Rows per view is set in Rules > Format (default 40). A member continues on the next page with a (contd.) header.
- Updating the same scope again reuses the same views, so their place on sheets is kept.
- When bars change, the views are marked out of date and a red OUT OF DATE banner appears the next time Revit is idle. Refresh BBS recalculates every BBS view from its stored scope (views made from a selection or the current view cannot be repeated and are listed).
- The view is ordinary Revit lines and text; models open and print the same without RebarIN.

## Native schedule
A Revit schedule named **RIN BBS** on Structural Rebar with Member, Bar mark, Shape code, Bar diameter, Quantity, Cutting length, Total length, Unit weight and Total weight, sorted by member then bar mark, with grand totals. Run Map shapes or the BBS first so the bars carry values. If it already exists it is opened unchanged.

## Rules
Tabs: **General** (name, cutting length method, rounding, stock length 6 to 18 m, unit weight source), **Bends and hooks** (per steel grade group: k, hook and bend allowances, minimum allowance, deductions at 45, 90, 135 and 180 degrees, 90 degree hook, 135 degree stirrup hook extension, stirrup and ring factors), **Laps**, **Unit weights**, **Format** (company, title, prepared by, checked by, date format, length unit, rows per view, wastage). Save to this project, to My rules (a file on your PC) or to the firm folder (add-on). Saving to the project recalculates the bars and marks BBS views out of date. Every value is validated.

## Try add on feature (optional subscription)
- **Export XLSX:** the schedule as an Excel file with member groups and the summary by diameter. Total bars, total length and total weight are Excel formulas. No Excel is needed on the PC. The ribbon button exports the whole model; the button in the BBS window exports what you see.
- **Firm rules:** choose a shared folder; save the project rules to it, load a firm rule set into a project, import a rule file, export the project rules, and lock the project rules. Rule files are named `Name.rinrules.json` and are checked before use.
- **Bar marks:** scheme `{n}`, `{memberPrefix}{n}` or `{dia}{n}`, a start number, keep or clear marks you typed, number per member or one running number. Applies to the whole model.

## Troubleshooting
- **Why does my cutting length differ from Revit's bar length?** RebarIN uses the allowance method with out-to-out dimensions, bend deductions and hook allowances. Switch the method to Revit centreline in Rules if you want Revit's length.
- **Why is a bar FF?** Its shape did not match a catalogue shape. Hooks and bends must be at 45, 90, 135 or 180 degrees to be recognised.
- **Some bars are "Not updated".** Another user owns them, or they are in a Revit group.
- **A shape failed to load.** See Load shapes: some shapes need a template.
- **A button is greyed out.** The commands need an open project.
- Rebar in linked models is not read. Multi-plane bars are listed as FF.

## Your data
The project rule set, the shape code on shapes RebarIN created and the settings of each BBS view are stored in your Revit model. Your own rule sets and settings are in `%AppData%\LeonS\RebarIN`; logs are in `%LocalAppData%\LeonS\RebarIN\logs`. Nothing is sent anywhere except the Autodesk licence check. To remove everything RebarIN stored in a model: ? > Data and diagnostics > Remove all RebarIN data from this model (rebar shapes and RIN_ parameters stay).

Privacy policy and licence terms: see the links on the app's store page, or the "Privacy policy" and "Licence terms (EULA)" topics in the ? guide.
