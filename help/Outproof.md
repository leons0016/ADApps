# Outproof guide

Outproof checks a Revit model before an IFC export and checks the IFC file after it. It never writes IFC itself: Autodesk's exporter stays the exporter. Support: atulhritik@hotmail.com

Autodesk and Revit are trademarks of Autodesk, Inc. IFC and buildingSMART are marks of buildingSMART International Limited. Outproof is an independent product of LeonS and is not affiliated with or endorsed by them.

## Plans
- Free for 24 hours from the first start, then a one-time purchase ($7) unlocks the export setups, the pre-flight report and the verification of an exported IFC file.
- Optional add-on ($2.5 per month, shown first on the ribbon as "Try add on feature"): comparison of two IFC files, CSV and XLSX reports, saved verification history.

## Quick start
1. **Export setups.** Press New, name the setup after the client or stage, choose the view definition (IFC2x3 Coordination View 2.0, IFC4 Reference View, IFC4 Design Transfer View) and the options. Press Save as new version, then Use for this model.
2. **Pre-flight.** Press Run pre-flight. Tick Measure geometry for the BREP prediction. Pick a finding to read it; press Select in Revit to select the elements it names.
3. **Export** with Revit's own exporter, or press Export now on the Verify tab.
4. **Verify.** Choose the IFC file and press Verify file.
5. **Keep the proof.** Save as text; with the add-on save CSV or XLSX, compare with the previous issue and keep the history.

## Export setups
A setup is a named, versioned set of export options: name, client or stage, notes, view definition, space boundaries, scope (entire model or active view), coordinate base, tessellation detail, base quantities, splitting walls and columns, Revit and IFC common property sets, user-defined property sets and their file, a class mapping file, linked files.
- Save as new version keeps the old version. Select a version to see what changed in it; Restore makes a new version with the old options.
- Use for this model: the pre-flight and the verification use this setup. The choice is remembered in the model.
- Export saves the selected setup or the whole library as a .json file; a colleague presses Import. A newer shared version replaces yours only if you have no unsaved edits; otherwise it is added as a separate copy "(imported)".
- The library is one file: `%AppData%\LeonS\Outproof\library.json`.

## Pre-flight checks
- **BREP prediction** (needs Measure geometry). IFC2x3 Coordination View 2.0 keeps strict extrusions parametric and writes everything else as BREP. A solid counts as an extrusion when one direction exists with a cap at each end and every other face parallel to it (or a cylinder around it). Walls, floors, roofs and ceilings with openings are given allowance for the openings the exporter writes separately. It is a screening rule; the verification measures how often it was right. IFC4 Reference View writes tessellated geometry for everything; Design Transfer View keeps parametric geometry where it can.
- **IfcExportAs and category mapping.** Elements with no IfcExportAs (instance or type) in a category that is neither in the exporter's default mapping nor in your mapping file are listed by category. Elements switched off (Do not export) are counted separately.
- **Shared parameters.** With user-defined property sets on, shared parameters that no property set definition names are listed.
- **Far from the origin.** Elements more than 5 km from the internal origin: warning; more than 32 km: error.
- **IFC add-ins.** The .addin manifests in the Revit add-in folders are read. Two IFC exporter registrations, two importer registrations, or one add-in id in two files are reported with the file paths.
- **IfcGUID.** Two elements with the same IfcGUID value, or a malformed value, are reported.

## Verify an exported IFC file
Every model element that is not switched off is looked up in the file by GlobalId, then by the Revit element id in the Tag attribute, and if that fails the check compares counts per IFC class only and says so.
- **Missing**: in the model, not in the file. **Only in file**: in the file, not in the model (parts the exporter creates itself, such as stair flights, railing parts, curtain wall parts and split wall pieces, appear here). **Different class**: not the IFC class that IfcExportAs or the mapping asks for.
- **File checks**: duplicated and malformed GlobalIds, schema and view definition against the setup, the farthest object from the origin, the exporter named in the header.
- **Geometry**: how many objects are BREP, extrusion or tessellated, and how many pre-flight predictions were right.
- **GlobalId rule**: the expected GlobalId is computed from the Revit UniqueId. Outproof tries three variants of how the element id is folded in and uses the one that reproduces the ids in your file; the figure shown is the share it reproduced.
- Missing rooms, spaces and grids are a note only: the exporter writes them only with particular options.

## Compare IFC files, reports and history (add-on)
- **Compare IFC files**: choose file A (older) and file B (newer). Objects are matched by GlobalId. New, removed and changed objects are listed; select a changed object to see each change (name, class, type, storey, moved by how many millimetres, geometry kind, every property whose value differs). Files from any program can be compared.
- **CSV and XLSX**: every report has CSV and XLSX buttons. The workbook has one sheet per table; values are stored as text so ids stay exact and nothing is read as a formula.
- **History**: each verification of this model is saved on this computer; the History tab shows them newest first with what moved since the previous one.

## What the checks cannot tell you
- Reading the model, measuring geometry, selecting elements and calling Revit's exporter depend on how your Revit version behaves; the Revit-side parts have not yet been run in a live Revit session.
- The BREP prediction is a screening rule. Hosted openings, nested families, groups and links can make it wrong.
- The mapping file and property set file layouts are read as the exporter writes them from memory of its layout; a line that is not understood is reported.
- Outproof is not a full IFC validator and does not check a file against the IFC schema or against a model view definition's rules.
- Elements of linked models, groups and nested components are not listed separately.

## Privacy and data
- Setups: `%AppData%\LeonS\Outproof\library.json`. History and settings: `%AppData%\LeonS\Outproof`. In the model: a short note of the setup it uses (Project Information element).
- Nothing is sent anywhere except the store licence check. IFC files are read on your computer.
- Logs: `%LocalAppData%\LeonS\Outproof\logs`. "Remove all Outproof data" (in the ? guide) removes the note in the model, the history and the settings; the setup library stays.
