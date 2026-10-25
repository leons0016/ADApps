# FlowPilot - git flow, release and stacked branches for JetBrains IDEs

Help page for the FlowPilot plugin (publish as `help/J1-FlowPilot.md` in the public repository).
Vendor: LeonS. Support: atulhritik@hotmail.com

## Getting started

1. Install **FlowPilot** from the JetBrains Marketplace (Settings > Plugins > Marketplace). No restart is needed.
2. Open a project that contains a Git repository. A **FlowPilot** tool window appears at the bottom (it does not open by itself).
3. FlowPilot chooses the workflow of each repository in this order:
   1. a valid `.flowpilot.yml` in the repository root (a team workflow),
   2. the git flow settings in `.git/config` (`gitflow.branch.master`, `gitflow.branch.develop`, `gitflow.prefix.*`),
   3. GitHub flow: feature branches off the main branch (detected from `origin/HEAD`, then `main`, then `master`).
4. Use **Start...** to create a branch from the right base and **Finish** to merge and clean up. Everything is also in the **Git > FlowPilot** menu
   and in Find Action (Ctrl+Shift+A / Cmd+Shift+A). FlowPilot ships no default keyboard shortcuts; bind your own in Settings > Keymap.

### Initialize git-flow

Git > FlowPilot > Initialize git-flow... asks for the production branch, the development branch and the prefixes, creates the development
branch if needed and writes the standard `gitflow.*` keys into the repository's local git config.

### Start and finish

* **Start**: type, name, base (read-only, from the workflow), "Fetch base first", "Switch to new branch", and what to do with uncommitted changes.
  If the local base branch is behind its remote branch, the new branch starts from the remote branch and FlowPilot tells you.
* **Finish**: a checklist of the planned steps with the exact git command under each one. Untick steps you do not want. Merge modes: `--no-ff`,
  `--ff-only`, `--squash`, rebase then fast-forward. On a merge conflict the run pauses, the IDE's conflict resolver opens and a banner offers
  **Continue** or **Abort**. A failed step can be retried or skipped, even after restarting the IDE.
* Pre-checks stop a finish early with a clear message: uncommitted changes, branch behind its remote branch, missing target branch, a merge or rebase
  already in progress, a tag that already exists.

## Coming from Git Flow Integration or Git Flow Integration Plus

Uninstall the old plugin (Settings > Plugins > Installed) and install FlowPilot. FlowPilot reads the `gitflow.*` settings already stored in your
repositories, so no migration is needed. The Start and Finish actions create the same kinds of branches (`feature/`, `release/`, `hotfix/`, ...).
FlowPilot is a separate product and is not affiliated with those plugins.

## Team workflows: `.flowpilot.yml`

Commit this file to the repository root. It must be smaller than 64 KB. Invalid files are reported with a line number in an editor banner and
FlowPilot falls back to the git flow / GitHub flow workflow.

```yaml
version: 1                       # required
name: "Release train"            # shown as "Team: Release train"
mainBranch: main                 # required
developBranch: develop           # omit for trunk-based workflows
remote: origin
tagPrefix: "v"
versionFiles:                    # used by bumpVersion steps; the first readable file gives the current version
  - path: gradle.properties
    kind: properties             # properties | json | xml-maven | toml | regex
    key: version
  - path: app/package.json
    kind: json
    key: version                 # top-level key only
  - path: VERSION
    kind: regex
    pattern: '^(?<v>\d+\.\d+\.\d+)$'
branchTypes:
  - id: feature                  # [a-z][a-z0-9-]*, unique
    prefix: "feature/"           # prefixes must not overlap
    base: develop                # a branch name, ${mainBranch} or ${developBranch}
    nameRegex: '^[a-z0-9][a-z0-9._-]{1,60}$'
    finish:
      - merge: { into: develop, mode: no-ff }     # no-ff | ff-only | squash | rebase-ff
      - deleteLocal: true
      - deleteRemote: ask                         # true | false | ask
  - id: release
    prefix: "release/"
    base: develop
    start:
      - bumpVersion: { default: minor }           # patch | minor | major | none
    finish:
      - merge: { into: main, mode: no-ff }
      - tag: { name: "${tagPrefix}${version}", annotated: true, message: "Release ${version}" }
      - merge: { from: main, into: develop, mode: no-ff }
      - deleteLocal: true
      - push: { branches: [main, develop], tags: true, ask: true }
```

Steps: `fetch`, `merge`, `rebase`, `tag`, `bumpVersion`, `deleteLocal`, `deleteRemote`, `push`. Variables: `${name}`, `${branch}`, `${version}`,
`${mainBranch}`, `${developBranch}`, `${tagPrefix}`. A custom type counts as Free only if its id is `feature` or `bugfix` and it uses only
`fetch`, `merge`, `deleteLocal`, `deleteRemote` and `push`.
When the YAML plugin and the JSON module are present, the editor offers completion and validation from the bundled JSON schema.

## Stacked branches

* Set a branch's parent with **Stacks > Set Parent...** or create a child with **New Branch on Top**. The parent and its commit at that moment are stored in
  `.git/config` (`branch.<name>.flowpilot-parent` and `flowpilot-base`), so they survive IDE reinstalls and work from the command line too
  (`git config --get-regexp flowpilot`).
* The tree shows ahead/behind against the parent and a warning icon when the parent has moved on.
* **Restack** rebases each child onto its parent in order using `git rebase --onto <new parent tip> <recorded base> <branch>`, so only the branch's own
  commits are replayed even when the parent was rewritten or squash-merged. On a conflict it pauses: resolve, then **Continue**, **Skip Branch**
  (the branch and its children stay unchanged) or **Abort** (everything is restored). Restack never force-pushes.
* When a branch is deleted through FlowPilot, its children move up to its parent.

## Search history for text

Find the commits that added or removed a string (`git log -S`, exact count change) or whose diff matches a regular expression (`git log -G`).
Case-insensitive search, paths, a range (HEAD, all branches or a ref) and dates are available; Stop cancels the git process. Double-click a row to open the
commit's files in the IDE diff viewer.

## Free, Pro and Plus

See the table in the store listing. In short: Free covers feature and bugfix flows, branches overview, stack viewing, Initialize git-flow and 200 search results.
**Pro** (one-time purchase, 7-day trial through the IDE licence dialog) adds release and hotfix flows, team workflows, restack, Changes vs Base and unlimited
search results. **Plus** (optional monthly add-on, a second plugin) adds branch workspaces, advanced search filters and the force-with-lease push helper.
Free features never expire and never show countdowns. If a licence ends, nothing in your repositories is changed and a paused run can still be continued or aborted.

## Troubleshooting

* **Git executable not found**: set it in Settings > Version Control > Git.
* **Merge or rebase conflicts**: FlowPilot pauses and opens the IDE's conflict resolver (Git > Resolve Conflicts). Resolve all files, then press Continue in the banner.
* **Push rejected / protected branch**: the remote decides. The notification shows git's message; the step is marked failed and can be retried or skipped.
* **Offline**: when fetching fails FlowPilot asks whether to continue without fetching.
* **"Parent branch ... no longer exists"** during restack: use **Set Parent...** to choose a new parent.
* **Pro locked right after buying or starting a trial**: Settings > Tools > FlowPilot > Refresh licence (otherwise the next check runs within 5 minutes).
* **Remove FlowPilot metadata**: Settings > Tools > FlowPilot > Clean up repository metadata (removes only the flowpilot-parent / flowpilot-base keys).

## Privacy

FlowPilot runs entirely inside the IDE and does not collect or transmit personal data. Details: the privacy policy linked on the listing.

## Changelog

* 2026.1.0 - first release (see the store listing change notes).
