# Sheetwick help

Sheetwick is a drawing issue register and transmittal record for Revit 2025, 2026 and 2027. It saves the whole sheet set at every issue inside your model (sheet number, title, revision, revision date and status, the purpose and date of the issue and which drawings were sent), builds the issue matrix from those records, and exports it. Publisher: LeonS. Support: atulhritik@hotmail.com.

> Sheetwick keeps a record of what you tell it was issued. It does not check drawing content or decide what may be issued. The layouts and presets are starting points: check them against your BIM execution plan, QA manual and contract. Check every register and transmittal before it is sent.

The same guide is built into Revit: click the **?** button on the Sheetwick tab.

## Plans
- Free for 24 hours from the first start (every feature).
- Then a **one-time purchase of $5** unlocks everything except the add-on.
- An optional **add-on at $2.5 per month** unlocks three extras, shown first on the ribbon under **Try add on feature**: Transmittal and issue sheet, Compare issues, Preset library.
- If you stop the add-on, your recorded issues, register layouts and drafting views stay. Only those three features stop.

## Quick start
1. **Open Sheetwick.** Sheetwick tab > Sheetwick. On Settings, type the sheet parameter that holds your status (or suitability) and the one for discipline, then Save settings. Both are optional.
2. **Record an issue.** Issues tab > New issue. Check the number and date, write the purpose, tick the drawings that go out, Record issue. Or click Import from Revit revisions to start from the revisions already in the model.
3. **See the register.** Register and matrix: one column per issue. Export XLSX or CSV, or Place on sheet to draw it in the model.

## Where to find what
| Panel | Buttons |
|---|---|
| Try add on feature | Transmittal, Compare issues, Preset library (optional subscription) |
| Register | Sheetwick (the window), New issue, Export register |
| Help | ? (guide, plans and licence, privacy policy, licence terms, data and diagnostics) |

The buttons need an open project (not a family).

## Columns and patterns
Each column of a register layout is a pattern: text with `{fields}`. Examples: `{number}`, `{discipline}-{number}`, `{param:Drawn By}`.

Fields: `number name rev revdate revdesc status discipline consultant source issue issuedate purpose issuedby project projectno client originator` and `param:Name` for any sheet parameter.

Modifiers go after a bar: `upper lower title trim nospace clean prefix digits suffix left:N right:N pad:N def:text`. Chain them: `{number|prefix|upper}`. A value map replaces whole values, for example `Work in progress = S0`.

Group by and Sort by are patterns too. Numbers sort the way people read them (A2 before A10). An issue column can show the revision, the issue date, revision and date, or a mark.

## External (consultant) drawings
On the External drawings tab type the number, title, discipline, consultant, revision and status of drawings made outside Revit. They are recorded in every issue and have rows in the register. No dummy sheets are needed.

## Add-on features
- **Transmittal.** Pick an issue and a layout (issue sheet or US transmittal), the recipients, the method and notes. Save XLSX or CSV, or export a folder with the transmittal and a PDF of every sent sheet (Revit's own PDF export, from the sheets as they are in the model now; sheets that changed since the issue are listed).
- **Compare issues.** Pick two issues. Drawings added, removed, with a new revision, renumbered, retitled or with a new status. Save as XLSX or CSV.
- **Preset library.** ISO 19650 information register, US drawing log, UK register with revision matrix, issue sheet and US transmittal layouts. Presets are copied into the project as your own layouts. Import and export template files and share them through a firm folder.

## Where your data lives
Recorded issues, settings, layouts and external drawings are stored inside the Revit model. Per-user settings are in `%AppData%\LeonS\Sheetwick`. Nothing is sent anywhere except the store licence check. Remove everything with **? > Data and diagnostics > Remove all Sheetwick data from this model**.

## Limits
- Revit does not keep a list of what was sent; Sheetwick records it when you click Record issue.
- Linked-model sheets are not read.
- A very large issue is compressed in the model; one above 3 MB is refused.
- The matrix drawn in Revit is plain lines and text. A matrix wider than the sheet is reported when it is placed.
