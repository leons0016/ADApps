# LaunchDeck help

LaunchDeck starts your local stack from one text file, `.launchdeck.yaml`, in the IDE you already use. Vendor: LeonS. Support: atulhritik@hotmail.com

Contents: Getting started - What is a deck - YAML reference - Readiness checks - Hooks - Environment and profiles - Command Line run configuration -
Before-launch task - Keyboard shortcuts (Plus) - Graph and restart policies (Plus) - Free, Pro and Plus - Security and trust - Coming from Compound
configurations - Troubleshooting - Uninstalling

## Getting started

1. Open the **LaunchDeck** tool window (bottom). With no `.launchdeck.yaml` it offers **Create deck from run configurations...** (tick your run
   configurations, order them with the arrows, name the deck; each one then depends on the one above it with a placeholder `delay: 2s` check)
   and **Create empty .launchdeck.yaml**.
2. Edit the file. Completion and error banners (with line numbers and "did you mean" hints) work in the editor; a green play icon next to a deck or
   node name starts it.
3. Pick the deck (and a profile, if the file has any) in the tool window and press **Start**. The first time (and whenever the file changed), LaunchDeck
   shows every command in the file and asks you to trust it.

## What is a deck

A deck is a named group of **nodes** (services). A node has one kind, optional dependencies and an optional readiness check. LaunchDeck starts nodes in
dependency order, nodes that do not depend on each other start in parallel (at most 8 at once). A node that fails blocks only the nodes that depend on
it. Stop goes in reverse order: a soft stop first, then after `stopTimeout` (default 10 s) the whole process tree is killed.

Status lights: grey stopped, blue starting, yellow waiting for readiness, green ready, red failed or blocked, orange restarting.

## YAML reference (schema version 1)

```yaml
version: 1                    # required
defaults:
  timeout: 120s               # readiness timeout (Settings value is used when absent)
  stopTimeout: 10s
  shell: default              # default | bash | zsh | sh | pwsh | cmd | none  (for shell nodes)
envFiles: [".env"]            # for every deck
env: { SPRING_PROFILES_ACTIVE: local }
profiles:
  dev:  { envFiles: [".env.dev"] }
  test: { env: { DB_NAME: app_test } }
decks:
  local:
    title: "Local stack"
    envFiles: []              # optional, deck level
    env: {}
    direnv: false
    nodes:
      db:
        compose: { file: "docker-compose.yml", service: postgres, profile: dev }
        ready: { port: 5432 }
      api:
        runConfig: "Spring Boot API"            # exact IDE run configuration name, or { name: ..., debug: true }
        dependsOn: [db]
        ready: { log: "Started .*Application in" }
        timeout: 180s
        restart: { policy: on-failure, maxRetries: 2, backoff: 3s }   # Plus
      web:
        npm: { script: dev, cwd: "frontend", args: [] }
        dependsOn: [api]
        env: { VITE_API: "http://localhost:8080" }
        ready: { http: { url: "http://localhost:5173" } }
        reuseIfPortBusy: true
      migrate:
        gradle: { tasks: ["flywayMigrate"], args: [], cwd: "." }
        dependsOn: [db]
        oneShot: true                            # success = exit code 0; dependents wait for the exit
    hooks:
      onReady:
        - openBrowser: "http://localhost:5173"
        - runConfig: { name: "Attach API debugger", debug: true }
afterLaunch:
  - runConfig: "Spring Boot API"
    when: { port: 8080 }
    do: [ { notify: "API is up" } ]
```

Node kinds: `runConfig`, `shell` (a string or `{ cmd, cwd, shell }`), `npm` (package manager from the lock file: `pnpm-lock.yaml`, `yarn.lock`,
`bun.lockb` / `bun.lock`, otherwise npm), `gradle` (`gradlew` / `gradlew.bat` in the working directory, otherwise `gradle`), `compose`
(`docker compose -f <file> up <service>`, attached so the logs stream; stop runs `docker compose -f <file> stop <service>`).
Common fields: `dependsOn`, `env`, `envFiles`, `direnv`, `ready`, `timeout`, `stopTimeout`, `restart`, `oneShot`, `reuseIfPortBusy`, `enabled`, `description`.
Names (decks, nodes, profiles) use letters, digits, `_`, `-`, `.` (max 64). Durations: `500ms`, `5s`, `2m`, `1h`. Limits: file 64 KB, 100 nodes per deck.
Values that become command-line arguments (npm script, gradle tasks, compose service/profile) must be plain tokens, so a field can never inject a
shell command; only `shell` nodes, hooks of type `shell` and Command Line configurations run text through a shell, by design.

## Readiness checks

| Check | Ready when | Tier |
|---|---|---|
| `port: 8080` | a TCP connect to 127.0.0.1:8080 (also ::1) succeeds; `{ port: 80, host: example.local }` for another host | Free |
| `delay: 5s` | the time has passed | Free |
| `http: url` or `{ url, status: 200 or "200-299" }` | a GET answers with the status (default any 2xx or 3xx), 2 s per attempt | Pro |
| `log: "regex"` | an output line matches (ANSI codes removed; lines printed before the check started are included) | Pro |
| `file: "build/ready.flag"` | the file exists (relative to the node's working directory) | Pro |
| several keys | all must pass (AND); free when all parts are free | |
| `allOf: [...]`, `anyOf: [...]` | all / one of the listed checks | Pro |

Polling: port and file every 500 ms, http every 1 s. If the process exits first the node fails with "exited with code N before becoming ready". On timeout
the node fails with "Not ready after N s: waiting for ..." and the process keeps running so you can look at it. A port check also runs a pre-check: if the
port is already open you can **Reuse it**, **Start anyway** or **Cancel deck** (or set `reuseIfPortBusy: true`).

## Hooks (Pro)

`hooks.onReady` runs in order when every node is ready. `afterLaunch` runs when you start a run configuration yourself (toolbar Run button), after its `when`
check passes, once per start; matching is by exact name (the first in Run Manager order if there are duplicates). Hook types: `openBrowser` (http or https
only), `runConfig` (`debug: true` uses the Debug executor), `notify`, `shell` (60 s limit, output goes to the deck log). A failing hook shows a warning and
never stops the deck. `afterLaunch` hooks only run for an approved deck file in a trusted project.

## Environment and profiles

Precedence, low to high: system, file `envFiles`, file `env`, deck `envFiles` / `env`, direnv, profile `envFiles` / `env`, node `envFiles`, node `env`.
`${VAR}` and `${VAR:-default}` expand from the values collected so far; `$$` is a literal dollar sign. `.env` syntax: `KEY=VALUE`, `export KEY=VALUE`,
`'literal'`, `"escapes \n \t"`, comments, multi-line values in double quotes. Paths are relative to the project. `direnv: true` (Pro) runs
`direnv export json`; "direnv is blocked" fails the node with a hint to run `direnv allow`. For `runConfig` nodes the values are added to a temporary copy of
the configuration if its type supports environment variables; otherwise a message says it runs with its own settings. Profiles are chosen in the tool window (Pro).
LaunchDeck's own log lines mask values of names containing secret, token, password or key.

## Command Line run configuration (Free)

Run | Edit Configurations | + | Command Line. Fields: command (several lines allowed), working directory (`$ProjectFileDir$`, `$USER_HOME$`), shell
(Default = `$SHELL -l -c` on macOS/Linux and `cmd /d /s /c` on Windows; bash, zsh, sh, pwsh, cmd, or None to run the program directly), environment variables
with "include system environment", ordered env files (later wins; the table wins over files), "Load direnv" (Pro), "Run in background" (asks the IDE not to bring the Run
window to the front; best effort), "Ready when" (Pro: used when the configuration is a deck node without its own `ready`).
Stop kills the whole process tree. It can be stored as a project file (`.run/*.run.xml`), used inside Compound configurations and as a before-launch step.

## Before-launch task (Pro)

Edit any run configuration | Before launch | + | **Start LaunchDeck deck**: deck, profile, wait until all nodes are ready (default on), timeout (default 180 s),
stop the deck when the configuration stops. If the deck is already ready it is reused.

## Keyboard shortcuts, graph, restart policies (Plus)

Settings | Keymap | search "LaunchDeck": Start / Stop / Restart deck `<deck>` and Start node / Restart node `<deck>/<node>`. Action ids are stable
(`LaunchDeck.Start.<deck>`, `LaunchDeck.RestartNode.<deck>.<node>`), so bindings survive edits. The **Graph** tab draws the dependency graph. `restart:` restarts
an unexpectedly exited service after `backoff x attempt` and checks readiness again; a service you stopped yourself is never restarted; after `maxRetries`
(a streak resets after 60 s of being ready) it is marked failed, and with `stopDependentsOnFailure: true` its dependents are stopped.

The Plus plugin also adds **Tools | Export LaunchDeck Graph...** (the dependency graph of a deck as a Mermaid diagram, Graphviz DOT or an SVG image, to the
clipboard or a file) and **Tools | LaunchDeck Service Monitor...** (process id, process count in the tree, CPU time and uptime of the running services).

## Free, Pro and Plus

See the table on the Marketplace page. Free never expires. Pro is a one-time purchase with a 7-day trial; Plus is a separate optional monthly plugin
("LaunchDeck Plus - Graph View"). A deck that uses a locked feature is shown read-only with the message "This deck uses Pro features (...). Start with Pro, or
start nodes one by one."; single nodes can always be started on their own without their checks. If a licence lapses, running services keep running.

## Security and trust

A deck file runs commands. LaunchDeck never starts anything by itself. When the file is new or changed, the first start lists every command, hook,
env file and tool it can run and asks for confirmation (a SHA-256 fingerprint of the approved file is stored in the project's workspace file). If the project
is not trusted, Start is disabled. LaunchDeck makes no network calls of its own except the http checks you write.

## Coming from Compound configurations

A Compound configuration starts everything at once. Create a deck from the same configurations (tool window button), then replace the `delay` placeholders
with real checks and add `dependsOn` where one service needs another.

## Troubleshooting

* **"Command not found: npm. Is it on PATH for GUI apps?"** On macOS an IDE started from the Dock does not see your shell PATH; start the IDE from a terminal,
  or use a shell node (`shell: "npm run dev"`), which runs through a login shell.
* **Windows:** commands run through `cmd /d /s /c` by default; use `shell: pwsh` for PowerShell. The `npm` kind runs `npm.cmd`. Paths in YAML accept `/`.
* **"Docker is not running. Start Docker Desktop and retry."** The compose node failed because the Docker daemon is not reachable.
* **"direnv is blocked here"** Run `direnv allow` in that folder.
* **Run configuration not found** The `runConfig` name must match exactly; a warning appears in the editor.
* **Nothing happens on Start** Check the banner: errors in the file, a cycle, or an untrusted project disable Start.

## Uninstalling

Unloading the plugin stops the services it started (after asking if any are running). `.launchdeck.yaml` stays. Command Line run configurations stay in the
project files and show as an unknown type until LaunchDeck is installed again.
