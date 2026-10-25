# RefactorLens - help

Semantic diff for JetBrains IDEs. Vendor: LeonS. Support: atulhritik@hotmail.com

## How it works

When you open a diff of a Java, Kotlin or Python file in the IDE (commit, Git log, compare with branch, shelf, local changes),
RefactorLens sends the two versions to its analysis engine, a small Java program that runs on your computer next to the IDE. The engine
is the open-source RefactoringMiner library. It reports the refactorings it detected: renames, extracted and inlined methods, moved
methods and classes, extracted variables, and so on. RefactorLens then checks every changed block of the diff against that report
and gives it exactly one class:

| Class | Badge | Meaning |
|---|---|---|
| Logic | none (optional `L`) | A real change, or a change RefactorLens cannot explain. The default. |
| Refactor | `R` | Fully explained: the block only renames identifiers the miner reported, or consists of code that the miner says was extracted, inlined or moved and that is found unchanged, in order, in the other version. |
| Format | `F` | Same tokens: only whitespace, line breaks, or import order differ. Python indentation is part of the code and never counts as format. |
| Moved | `M` | A deleted block and an inserted block of at least 3 lines with identical tokens (Pro). |

**Why is something marked Logic?** Because RefactorLens never hides a change it cannot explain ("when in doubt, logic"). A rename
mixed with another edit on the same lines, a changed constant inside moved code, a rename inside a string, a refactoring the
engine did not report, or an engine error all end up as Logic.

## Supported languages

Java and Kotlin (free), Python (Pro). The engine parses the code itself, so no language plugin is needed. JavaScript, TypeScript, C and
C++ are not offered yet. Quality differs by language: the miner's own README states that its detection precision for Kotlin has not been validated, and Java is its main target.

## Refactoring types shown

Everything the miner reports is listed in the banner (for example Rename Method / Variable / Parameter / Class, Extract Method,
Inline Method, Move Method, Pull Up Method, Extract Variable, Change Parameter Type ...). Only renames, extract, inline, move,
pull up and push down can make a block "Refactor"; the other types (such as Change Return Type or Add Parameter) are listed but never hide code,
because they change behaviour. Extract Variable and Inline Variable are listed but their lines stay Logic in this version.

## The banner

* Collapsed: summary, the first three refactoring types, Details, the review-mode check box (with a PRO badge when locked), and a menu
  (Re-analyse, Copy summary, Settings).
* Details: the list. Click an entry to scroll both sides to its code and flash it. Free: the first 20 entries.
* Messages: analysing; file too large (default limit 1,000 KB, setting 100-5,000); stopped after the timeout (default 20 s, "Re-analyse with
  60 s"); engine could not start (with the reason, Show log, Settings); Python on the free plan (a chip with "Start free trial").
  Unsupported file types show nothing.

## Review mode (Pro)

The toolbar toggle "Hide Refactor & Format Changes" (or the banner check box) folds every Refactor, Format and Moved block on both sides
into a one-line note and mutes its colours. Click a note to expand just that block. The state is remembered. "Next Logic Change" /
"Previous Logic Change" in the diff context menu visit only Logic blocks. No default keyboard shortcut is set; add one in Settings > Keymap.
If you use side-by-side view and the two sides scroll out of step around folded blocks, use the Unified viewer for review mode.

## Reports (Plus)

From the Git log (right-click a commit, or select several commits): **RefactorLens: Analyse Changes**. From the Git menu >
RefactorLens: **Analyse Local Changes** and **Compare with Base and Analyse**. The **RefactorLens** tool window shows the summary,
the file table (logic-heavy files first, filter "Only files with logic changes"), the refactorings grouped by type, progress with Cancel
(a cancelled analysis keeps its partial result, marked "partial"). Double-click a file or a refactoring to open its diff. **Export**
writes Markdown or HTML; **Copy as Markdown** puts it on the clipboard for a pull request description. All files of a change set are
analysed together in chunks of at most 200 files or 10 MB, so cross-file moves are found inside one chunk only. Generated folders
(`build`, `generated`, `out`) are skipped.

## Settings (Tools > RefactorLens)

Enable in diff viewers; languages; hide by default (Pro); imports as format; badge logic changes; max file size; analysis timeout;
engine memory (512 MB, 1 GB, 2 GB, 4 GB); stop engine after idle (5, 15, 60 min, never); disk cache; optional Java executable for the
engine; engine status with Restart engine and Open engine log; licence block.

## Performance and memory

The engine starts on the first diff (a few seconds), is shared by all projects of the IDE, runs at most two analyses at once (diffs
first, reports second), caches results and stops after the idle time. Its memory limit is a setting; an out-of-memory error is
reported with a hint and the engine restarts. If the engine crashes it is restarted up to 3 times in 10 minutes. The IDE itself
only holds the results.

## Privacy

Everything runs on your computer in a separate local process; nothing is uploaded; there is no telemetry. The cache holds analysis
results only (no source code) and can be switched off. See the privacy policy:
https://raw.githubusercontent.com/leons0016/ADApps/main/privacy/J5-RefactorLens.txt

## Third-party notices

The plugin includes open-source components (RefactoringMiner, MIT; GumTree, LGPL; Eclipse JDT, EPL; Kotlin compiler libraries, Apache
and others). They are plain jar files in the plugin's `lib/engine` folder with `THIRD-PARTY-NOTICES.txt` and the licence texts.

## Limits (honest list)

* Only two-side text diffs are handled; three-way merge, binary and image viewers are ignored.
* The engine needs Java 21 or newer, taken from the IDE; the miner jar is published for Java 23 and its class file version was lowered
  (see THIRD-PARTY-NOTICES).
* A file that does not parse on one side gives no refactorings; all its changes are Logic.
* Detection quality is the miner's; RefactorLens adds the safety rule but cannot make the miner find more than it finds.

## Support

atulhritik@hotmail.com
