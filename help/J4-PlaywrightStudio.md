# Tracelark - help

Independent plugin that works with Playwright. Not affiliated with or endorsed by Microsoft. Playwright is a trademark of Microsoft.
Vendor: LeonS. Support: atulhritik@hotmail.com

## Getting started (JavaScript / TypeScript)

1. Open a project that contains `playwright.config.ts` (or `.js`, `.mjs`, `.cjs`) and trust it (the config is code and is executed to list tests).
2. Install Playwright in the project: `npm install -D @playwright/test` and `npx playwright install` (the plugin never downloads browsers itself;
   if they are missing, the panel offers "Install browsers").
3. Open the **Playwright** tool window (right side). The tree appears after Playwright has listed the tests.
4. Run: right-click a node > Run, or click the run icon in the editor gutter, or *Run Test at Caret* in the editor menu.

Minimum Playwright version: 1.40. Capabilities such as `--last-failed`, `--only-changed`, `--repeat-each`, `--ui` and `--add-reporter` are detected from
`playwright test --help` of the installed version; actions that need a missing flag tell you so.

## Getting started (Python, pytest-playwright)

1. Install: `pip install pytest pytest-playwright` (in a virtual environment inside the project, `.venv` or `venv`, or set the interpreter in Settings).
2. The project is detected when `pytest.ini`, `pyproject.toml`, `setup.cfg`, `tox.ini` or a requirements file mentions playwright, or
   `conftest.py` imports it.
3. Discovery uses `python -m pytest --collect-only -q`; runs use `python -m pytest <node ids> -p pwstudio_pytest`. Browser, headed and tracing
   options map to the pytest-playwright options.

## The tool window

* **Config** combo (only with several configurations), **Refresh**, **Run All**, **Run Last Failed** (Pro), **Run Changed** (Pro), **Watch** (Pro),
  **Open Report**, **UI Mode** (Pro), **Start Dev Server** and **Flaky Test History** (both from the Plus add-on), **Settings**, **Help**.
* Project chips: Free selects one project or All; Pro selects several. Text filter matches titles and tags; the tag combo lists tags found in the tests.
* Context menu: Run, Run Headed, Run in Project, Debug with Inspector, Debug in IDE (Pro), Find Flaky Tests (Plus), Open Trace (Pro), Copy Grep Pattern, Jump to Source.
* Icons: green passed, red failed, yellow flaky (passed on retry), grey skipped.

## Run configurations

*Run > Edit Configurations > + > Playwright.* Scope (all, file, test, grep), projects, headed, workers, retries, trace mode (Pro), extra CLI args,
environment variables, interpreter and working directory. The grep for JavaScript is a regular expression; for Python it is a `-k` expression.

## Traces and reports

* **Open Trace** (Pro) runs `playwright show-trace <trace.zip>` and opens Playwright's own Trace Viewer window. Run with trace *On* or *Retain on failure*
  (toolbar trace mode, or `use.trace` in the config) so that a trace exists. For Python the trace is found in the pytest-playwright output folder.
* **Open Report** runs `playwright show-report` on a free local port for the `playwright-report` folder. The plugin keeps HTML reporters from waiting for Ctrl+C
  (`PLAYWRIGHT_HTML_OPEN=never` for runs started by the plugin).

## Watch mode (Pro)

Switch **Watch** on. After a save and a quiet period (default 800 ms) the plugin reruns the saved spec files, or the last run's scope when other
files were saved. A newer save cancels a running watch run. Folders such as node_modules, test-results and playwright-report are ignored.

## Recording (Plus add-on)

Right-click in a test > Playwright > *Record at Caret...* > start URL, browser, optional device and test id attribute. Playwright's recorder opens; when
you close it the steps are inserted at the caret in one undoable step. A missing `expect` import is added for JavaScript files.

## Dev server keeper (Plus add-on)

Settings > Dev server command and ready URL (defaults: the `dev` or `start` script, the config's `webServer.url`, else `http://localhost:3000`). The server runs
in its own tab and is stopped when you press Stop, close the project or unload the plugin. Runs reuse it when your config allows
`reuseExistingServer`.

## Flaky finder and history (Plus add-on)

* Right-click one or more tests > **Find Flaky Tests (Repeat x N)...** and enter a number from 2 to 200. The plugin runs them with `--repeat-each N` and
  retries off, so every failure counts. When the run ends a report lists each test: runs, how many did not pass, a verdict and a trail of letters
  (P passed, F failed). **Flaky** means it both passed and failed; **Always failing** means it failed every time (that is a bug, not flakiness).
* **Tools > Playwright > Flaky Test History...** shows the last 20 outcomes of every test and project from your normal runs, kept on your computer in the
  IDE system folder. K means the test passed only after a retry. **Clear History** deletes it.
* The history records while the add-on licence is active or still being checked, and stops recording when it is definitely missing.

## Debugging

* **Debug with Inspector** (Free): `--debug`, headed.
* **Debug in IDE** (Pro): the IDE's Node.js debugger runs `cli.js` with `--workers=1 --timeout=0 --headed`; breakpoints in test code stop. Available in IDEs with the
  Node.js plugin (WebStorm, IntelliJ IDEA with the JavaScript plugin).

## Settings (Settings > Tools > Tracelark)

Node interpreter, package manager, Python interpreter, dev server command and URL, gutter icons (Automatic turns them off when the IDE has its own Playwright run markers),
watch debounce (300-5000 ms), discovery timeout (10-900 s), licence status.

## Troubleshooting

| Message | What to do |
|---|---|
| Node.js not found | Set the interpreter in Settings, or install Node.js and restart the IDE |
| Playwright is not installed in this project | Press *Run npm install* (or install `@playwright/test` yourself) |
| Playwright browsers are not installed | Press *Install browsers* |
| Trust this project to list Playwright tests | Trust the project in the IDE dialog; the config file is executed |
| Yarn Plug'n'Play project (no node_modules) | Detected through `.pnp.cjs`; the plugin uses `yarn playwright` |
| No tests found | Check `testDir` and `testMatch` in the config |
| Results tree incomplete | Use a Playwright version 1.40 or newer; older versions lack parts of the reporter API |
| Duplicate gutter icons in WebStorm | Settings > Gutter run icons > Off (Automatic does this when it detects the IDE's own Playwright support) |

## Compatibility

IntelliJ-based IDEs 2025.3 and newer. Remote Development is not tested. JavaScript/TypeScript tests need Node.js; Python tests need Python with pytest.
