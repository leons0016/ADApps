# Buildtide help

Buildtide times every Gradle and Maven sync and build you run from the IDE and keeps the numbers on your computer. Support: atulhritik@hotmail.com

## Quick start

1. Run a Gradle or Maven build or sync as usual (Gradle tool window, a run configuration, the sync button, Maven tool window).
2. Open the **Buildtide** tool window (bottom). **Builds** lists the runs of this project; select one to see its timeline and slowest steps.
3. **This month** shows how long you spent waiting on syncs and builds in the current calendar month, over all your projects.

Nothing has to be set up. Recording starts when the plugin is installed; runs that happened before are not known.

## What is recorded, and what the IDE gives us

| Signal | Source | Notes |
|---|---|---|
| Gradle sync and Gradle run: start, end, result | the IDE's external-system task events | every run started from the IDE |
| Gradle task start/end, executed or up-to-date, failed, skipped | the same events (per task, with the build's own timestamps) | test events are skipped |
| Gradle "from cache" | console text (`> Task :x FROM-CACHE`) and Gradle's `N actionable tasks: ...` line | the IDE does not pass this flag on as an event; builds that print nothing show 0 |
| Gradle configuration cache reused / not reusable and why | console text (`Reusing configuration cache`, `Configuration cache entry reused / stored`, `Calculating task graph as ... cannot be reused because ...`) | best effort; wording depends on the Gradle version; shown as "not reported" when absent |
| Maven build: modules and goals | the Build tool window events the IDE creates from Maven output | as far as the IDE reports them |
| Maven sync | the Sync tool window events | steps are listed as reported |
| Other tools in the Build tool window | same events | off by default (Settings) |

Not recorded: runs started in a terminal, by CI or by another tool; Gradle daemon internals; a dependency graph (so there is no computed
critical path, only what ran when).

## The Builds tab

The list shows the last 10 builds of the project (Pro: 100, Plus: all in the History tab). The right side shows the timeline: one bar per
step on a time axis, one row per parallel lane (blue executed, green from cache, grey up-to-date or skipped, red failed). Hover for details.
**Slowest steps** lists the longest tasks, goals or configuration steps. **By module** (Pro) adds up the stored steps per Gradle project or Maven module.

Only the 100 longest steps of a run are kept (runs older than 14 days keep 20). The counts of executed, up-to-date and from-cache tasks are taken
before that cut.

## This month

The figure is the wall-clock time of the syncs and builds that started in the calendar month. Runs that overlap in time (two projects building
at once) are counted once. It is the time the build ran, not proof that you were looking at the screen. Pro adds a bar chart of the busiest days
and **Copy Monthly Waiting Time** (one sentence you can paste into a message).

## Pro (one-time purchase)

- The last 100 builds in the list.
- Time per module, busiest days of the month.
- Copy a build summary or the monthly sentence as text.

## Buildtide Plus (optional monthly add-on, separate plugin)

Install **Buildtide Plus** next to Buildtide. It adds three tabs and an export button:

- **History**: every stored run with filters.
- **Trends**: week by week, tasks that changed between the last two weeks, and regressions. A regression is a task (or a whole run) whose median
  time over the latest 3 runs is at least 2 times (adjustable) the median of the runs before, by at least 2 seconds, with at least 3 earlier runs.
  Only runs that did real work are compared. You get one message per regression ("Task :app:compileKotlin regressed 3.3x since 2026-10-07").
- **Cache**: configuration cache reused / not reusable with reasons, tasks from cache, from the console text as described above.
- **Export Build Report...**: Markdown or one HTML file, written where you choose. Nothing is uploaded.

## Settings (Settings | Tools | Buildtide)

Switch recording off, record syncs and/or builds, Maven on/off, other builds on/off, ignore runs shorter than N milliseconds, how many runs and
days are kept (default 2000 and 365), delete all recorded builds, licence refresh.

## Where the data is

One text file `buildtide/ledger.jsonl` in the IDE's system folder (Help | Buildtide Status shows the path). Delete it or press
**Clear Recorded Builds** at any time. Nothing is sent anywhere.

## Troubleshooting

- *Nothing is listed.* Run a build from the IDE (not a terminal). Check Settings | Tools | Buildtide that recording is on and the minimum duration is small.
- *Cache says "not reported".* Gradle printed no cache lines (quiet log level, or caching is off). Add no flags just for Buildtide.
- *Maven shows only a few steps.* The IDE builds the Maven tree from Maven's console output; what it does not recognise is not listed.
- *Licence.* Pro and Plus are managed in the IDE's licence dialog; Settings | Tools | Buildtide has **Refresh licence**.

Buildtide is not affiliated with Gradle Inc. or the Apache Maven project.
