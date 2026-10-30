# TourDeck help

Code tours for onboarding: a tour is a file in your repository that walks a new joiner through the code, step by step, inside the IDE.
Support: atulhritik@hotmail.com

## Quick start

1. Open the **TourDeck** tool window (right side). The **Tours** tab lists the tours of the project (files in the `.tours` folder).
2. To follow a tour, double-click it (or select it and press **Start Tour**). The **Follow** tab opens, the code of the step is opened in the
   editor and the lines are highlighted with a mark in the gutter. **Next** and **Previous** move between steps. **Resume Tour** continues where you stopped.
3. To write a tour, press **Start Recording**, click through the code the way you would explain it, and press **Stop Recording**. A dialog lists the places
   you stopped at: untick the ones you do not want, rename the others, write a text for each step, name the tour, **OK**. The tour is saved to `.tours/<name>.tour`.
4. Commit the `.tours` folder. The tour is now reviewed in pull requests like code and every colleague gets it with `git pull`.

Without recording: put the caret on a line (or select lines) and use **Add Step Here** (Tools | TourDeck, the editor context menu, or the button in the tool window).

## How steps find their place

A step stores the file, the lines, the **named element** around them (class, method, function), the position of the lines inside that element, and the
first lines of code at that place. When you follow a tour, TourDeck finds the place in this order:

1. the named element, plus the same offset inside it (so adding lines above a method moves the step with the method),
2. the recorded line numbers, if the code there still matches,
3. the recorded code text, anywhere in the file (the nearest match).

If none of these finds the code, the Follow tab says so and shows the old position. Variables, parameters and anything inside a code block are not used as
anchors (their names repeat); the enclosing method is. In files without named elements (plain text, YAML, Markdown) steps rely on lines and text.
The free plugin does not tell you that a tour has gone stale; **TourDeck Plus** does (see below).

## Gutter marks

The first line of every step is marked in the editor gutter. Click the mark to start the tour at that step. Switch the marks off in Settings | Tools | TourDeck.

## Step texts

Step texts are Markdown: `**bold**`, `*italic*`, `` `code` ``, bullet and numbered lists, block quotes, code blocks (three backticks) and links.
A link to code is written `[the loop](src/main/Service.kt:42)` (path relative to the project folder, optional `:line`); it opens the file. `http`, `https` and `mailto`
links open in your browser. Raw HTML is shown as text. In a Markdown tour file, headings of level 1 and 2 are reserved for the tour and its steps; they are shifted down in a step text.

## Tour files

Files live in `<project folder>/.tours/`. Two formats, both readable in a pull request:

`*.tour` (JSON):

```json
{
  "version": 1,
  "id": "onboarding",
  "title": "Welcome to the order service",
  "description": "Read this first.",
  "ref": "3f2a9c1d...",
  "branch": ["main", "release/*"],
  "requiredFor": ["backend"],
  "steps": [
    { "title": "Start here", "description": "Text without code." },
    {
      "title": "The entry point",
      "description": "Explains **why** it works like this.",
      "file": "src/main/kotlin/Service.kt",
      "line": 12, "endLine": 18,
      "symbol": { "kind": "KtNamedFunction", "name": "run", "container": "Service", "line": 11 },
      "text": "fun run() {"
    }
  ]
}
```

`*.tour.md` (Markdown): front matter (`id`, `ref`, `branch`, `requiredFor`), `# Title`, a description, and one `## Step title` per step with an HTML comment line
`<!-- tourdeck file=src%2FService.kt lines=12-18 symbol=... text=... -->` holding the anchor (values URL-encoded), followed by the step text.
TourDeck writes both formats; use whichever you prefer to review. `ref` is the commit the tour was recorded at. `branch` and `requiredFor` are used by TourDeck Plus.

Rules for tour files (they come from a repository, so they are checked): the file must be under 2 MB; at most 500 steps; paths must be relative and stay inside the project
(no `..`, no drive letters); a file that cannot be read is listed in the Tours tab with the reason and is not followed.

## Limits, Pro and Plus

| | Free | Pro (one-time purchase, 7-day trial) | Plus (optional monthly add-on, separate plugin) |
|---|---|---|---|
| Read and follow tours, gutter marks, progress | unlimited | unlimited | unlimited |
| Recorder, Add Step Here, step editor, Markdown | yes | yes | yes |
| Tours per project you can have | 3 | 20 | unlimited |
| Steps per tour | 10 | 40 | unlimited (up to 500) |
| Export a tour as one Markdown document, duplicate a tour | - | yes | |
| Staleness check, one-click re-anchoring, git diff and blame | - | - | yes |
| Branch-aware tours | - | - | yes |
| Required tours per role, checklist and progress view | - | - | yes |

The limits count the tour files in the project, whoever wrote them. Tours above a limit (for example after a licence ends) stay readable and followable; only creating more is locked.

## TourDeck Plus

Install **TourDeck Plus** next to TourDeck. Its tabs appear inside the TourDeck tool window.

* **Staleness**: press *Check Tours*. Every step is compared with the code: *Moved* (the element or the code is elsewhere in the file), *Changed* (edited; the closest match is proposed),
  *File moved*, *File missing*, *Code missing*. With the Git plugin the check also uses the diff since the commit the tour was recorded at and shows who last changed the place (git blame).
  *Re-anchor Step* writes the proposed place into the tour file; *Re-anchor All Confident* does it for every step at or above the confidence set in Settings | Tools | TourDeck Plus.
  The change is a normal edit of the tour file: review it in the diff. *Mark Tour Up to Date* records the current commit in tours without stale steps. The check also runs once in the background when a project opens (switch off in settings).
* **Branches**: add `"branch": ["main", "release/*"]` to a tour (or fill *Branches* in Edit Tour). `*` matches any text, `?` one character. The Tours list marks tours that belong to another branch and can hide them.
  Tour files with the same `id` and different `branch` lists work as variants of one tour.
* **Checklist**: add `"requiredFor": ["backend"]` to a tour (`"all"` = everybody). Choose your role in the Checklist tab: required tours come first, with your progress and the percentage of required tours finished.
  *Copy Progress as Markdown* gives a table to paste into a ticket. A tour that changed after you finished it is shown as "done, tour changed since" and does not count as done.

## Good to know

* Tours are looked up in the folder `.tours` of the project directory (the first project folder). A step can only point to files inside the project.
* Progress is stored per person in the workspace file of the project (`.idea/workspace.xml`), not in the tour files.
* The recorder adds a step when the caret rests on a line for the time set in Settings (default 1.5 seconds); you can also switch automatic capture off and use only Add Step Here.
* The staleness check proposes; it cannot know what the author meant. Look at the proposals before you commit them.
* No AI: tours are written by people. TourDeck never sends code or tours anywhere and has no network access of its own.

## FAQ

**A step highlights the wrong place.** The code changed since the tour was written. Edit the step (Follow tab) or, with Plus, run the staleness check.
**Where is my progress?** In the workspace file of the project on your computer. Deleting it resets all progress.
**Can I use the tours without the plugin?** Yes: `.tour.md` files read as normal Markdown documents with the anchors in comments; use *Export as Markdown* (Pro) for a clean document.
**The Plus tabs are empty.** Plus is a separate plugin; install it, or open Settings | Plugins to check it is enabled.
