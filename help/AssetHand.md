# AssetHand help

AssetHand writes COBie 2.4 asset-handover workbooks from your Revit 2025, 2026 or 2027 model and checks them before you send them. Every problem names the element, the COBie rule and the column; click it to select the element in the model. Output is a real .xlsx file (no Excel needed) or one CSV file per sheet.

> AssetHand prepares and checks data. The information manager remains responsible for checking the workbook against the client's requirements before issue. Check the closed value lists (units, asset type, duration unit) against your client's version of COBie.

The same guide is built into Revit: click the **?** button on the AssetHand tab.

## Plans
- Free for 24 hours from the first start (every feature).
- Then a **one-time purchase of $7** unlocks everything except the add-on.
- An optional **add-on at $3.5 per month** unlocks three extras, shown first on the ribbon under **Try add on feature**: client mapping templates, delta export and the export audit log.
- If you stop the add-on, your templates, issues and log stay in the model. Only using them stops.

## Quick start
1. **Set up the project.** AssetHand tab > Set-up. Facility, project, site, phase, units, currency and contacts. The first contact is the author of every row; each contact needs an email, given name, family name and company.
2. **Add the COBie parameters (optional).** COBie parameters > Yes. Shared parameters named COBie.* are added to types, components and rooms. Without them AssetHand reads Revit's own Manufacturer, Model, Description, Mark, OmniClass Number and Assembly Code.
3. **Build and validate.** Export > tick linked models if needed > Build and validate. Click a problem to select the element. Double-click zooms to it.
4. **Export.** Export XLSX, or Export CSV for one file per sheet.

## Where to find what
| Panel | Buttons |
|---|---|
| Try add on feature | Templates, Delta export, Audit log (optional subscription) |
| COBie | Export, Set-up, COBie parameters |
| Help | ? (guide, plans and licence, privacy policy, licence terms, data and diagnostics) |

The buttons need an open project (not a family).

## How the sheets are filled
| Sheet | Source |
|---|---|
| Contact, Facility | Set-up window |
| Floor | Levels (same-name levels in linked models are merged) |
| Space | Placed rooms (or MEP spaces when the model has no rooms). Name = room number, RoomTag = room name |
| Zone | COBie.Space.Zone (or Zone) on rooms, separated by comma or semicolon; HVAC zones |
| Type | Each family type used. Name = Family:Type. Category from COBie.Type.Category, OmniClass or Assembly Code |
| Component | Elements of the chosen categories. Name = Type:Mark (or Type:ElementId). Space = the room it sits in |
| System | From the element parameter System Name |
| Attribute | Parameters named COBie.Attribute.* and attributes listed in a template |
| Coordinate | Written when a template asks for it |
| Assembly, Connection, Spare, Resource, Job, Impact, Document, Issue | No Revit source: empty unless a template adds static rows |
| PickLists | The value lists AssetHand checks against |

Manufacturer and warranty guarantors must be contact emails. If a parameter holds a company name that matches a contact's Company, the contact's email is used.

## The checks
| Id | Level | Check |
|---|---|---|
| CB-REQ | Error | A required column is empty |
| CB-DUP | Error | Two rows have the same name (spaces: same floor and name) |
| CB-REF | Error | A value points to a row that does not exist |
| CB-LIST | Error | A value is not in the closed pick list (units, asset type, duration unit) |
| CB-EMAIL | Error | A contact column does not hold an email address |
| CB-DATE | Error | A date is not ISO 8601 |
| CB-NUM | Error | A number column holds something else |
| CB-CUR | Error | The currency is not a three-letter code |
| CB-NAME | Error | A name contains a comma |
| CB-FAC | Error | The Facility sheet does not have exactly one row |
| CB-SHEET | Error | SheetName and RowName do not point to a COBie row |
| CB-AREA | Warning | A space has no area |
| CB-FLOOR | Warning | A floor has no space |
| CB-RANGE | Warning | A quantity is negative |

## Client templates (add-on)
A template says which parameter feeds which COBie column (parameters separated by |, first one with a value wins, `=text` is a fixed value, `{A}: {B}` joins values), which categories are included, which contacts exist, which checks apply, which columns are not required (or also required) and which values the client accepts. Templates are stored in the model; Export file and Import file share them as `.assethand.json` files.

## Delta export and audit log (add-on)
- Tick **Only changes since the last issue**. AssetHand compares the workbook with the last recorded issue of the same template and writes only added and changed rows, plus a Delta sheet that also lists deleted rows.
- An issue is recorded when you export with **Record as an issue** ticked. The audit log lists user, date, template, issue number and label, file name, rows, errors, warnings and changes. Save it as CSV.

## Troubleshooting
- *Every type has no Category.* Fill OmniClass Number or Assembly Code on the type, or COBie.Type.Category, or map your classification parameter in a template.
- *A component is not in any space.* It is outside every placed room, or the room is in a link you did not tick.
- *A manufacturer fails.* COBie wants a contact's email. Add a contact whose Company is the manufacturer's name.
- *Nothing to export for a delta.* Nothing changed since the last recorded issue.
- *Buttons are greyed out.* Open a project (not a family).

## Your data
The set-up, templates, audit log and issue snapshots are stored in your Revit model. Settings are in `%AppData%\LeonS\AssetHand`, logs in `%LocalAppData%\LeonS\AssetHand\logs`. Nothing is sent anywhere except the Autodesk licence check. Use **? > Data and diagnostics > Remove all AssetHand data from this model** to remove it.

## Contact
atulhritik@hotmail.com
