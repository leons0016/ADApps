# CoverLens help

CoverLens shows code coverage reports in the editor gutter of JetBrains IDEs. Support: atulhritik@hotmail.com

## Quick start

1. Run your tests with coverage in your usual way (CI, terminal, Docker, Gradle...). 
2. In the IDE open the **CoverLens** tool window (bottom) and press **Load report...**, or accept the balloon that appears when a known report file exists.
3. Open a source file: bars in the gutter show covered (green), partly covered (yellow) and not covered (red) lines.

## How to create a coverage report

| Tool | Command (example) | File |
|---|---|---|
| Jest | `jest --coverage --coverageReporters=lcov` | `coverage/lcov.info` |
| Vitest | `vitest run --coverage --coverage.reporter=lcov` | `coverage/lcov.info` |
| c8 | `c8 --reporter=lcov npm test` | `coverage/lcov.info` |
| pytest-cov | `pytest --cov=src --cov-report=xml` | `coverage.xml` (Cobertura) |
| coverage.py | `coverage xml` or `coverage json` | `coverage.xml` / `coverage.json` |
| Go | `go test -coverprofile=cover.out ./...` | `cover.out` |
| Gradle JaCoCo | `./gradlew test jacocoTestReport` (enable the XML report) | `build/reports/jacoco/test/jacocoTestReport.xml` |
| Kover | `./gradlew koverXmlReport` | `build/reports/kover/report.xml` |
| PHPUnit | `phpunit --coverage-clover clover.xml` or `--coverage-cobertura cobertura.xml` | the named file |
| coverlet (.NET) | `dotnet test --collect:"XPlat Code Coverage"` (Cobertura) | `coverage.cobertura.xml` |
| gcovr | `gcovr --cobertura -o coverage.xml` | `coverage.xml` |
| grcov (Rust) | `grcov . -t lcov -o lcov.info` | `lcov.info` |

Produce coverage on source files (source maps are not applied).

## Formats

lcov, Cobertura XML, JaCoCo XML (also Kover) are free. Clover XML, Go cover profile, istanbul `coverage-final.json` and coverage.py JSON need Pro. The format is detected from the first 4 KB of the file; use **Choose format** if detection fails.

## Path mapping

Reports made in Docker or CI contain foreign paths such as `/app/src/user.ts`. CoverLens finds the common prefix and maps it to your project (one automatic prefix mapping in Free). Pro adds a mapping table (first match wins), unique-suffix matching, Go module paths and JaCoCo package paths against all source roots. Files that could not be matched are listed under "Not found".

## Diff coverage (Pro)

Changes tab: lines added or modified against the merge-base of the base branch and HEAD, plus uncommitted changes (optional). Only executable lines (lines the report has data for) count; partly covered lines count as covered but are listed. Base branch: choose in the tab, or Settings, or `baseBranch` in `.coverlens.json`; otherwise detected (origin/HEAD, main, master, develop).

## Commit check (Plus)

This is a feature of the optional add-on plugin "CoverLens Plus" (install it next to CoverLens). The commit window warns when the lines in the commit have less coverage than the threshold. Silent when no report is loaded. Switch off in Settings or with threshold 0.

## .coverlens.json

```json
{
  "version": 1,
  "sources": [
    { "glob": "packages/*/coverage/lcov.info", "format": "auto", "root": "packages/${1}" },
    { "glob": "backend/build/reports/jacoco/test/jacocoTestReport.xml", "format": "jacoco", "sourceRoots": ["backend/src/main/java", "backend/src/main/kotlin"] },
    { "glob": "coverage.xml", "format": "cobertura", "mappings": [ { "from": "/app/", "to": "" } ] }
  ],
  "threshold": 80,
  "baseBranch": "origin/main"
}
```
`${1}` is the first `*` of the glob. Free users use the first source of the file that has a free format. A bundled schema gives completion when the JSON module is available.

## Limits and notes

* Reports above the size limit in Settings (default 200 MB) are refused. Reports are streamed, never built as a tree.
* XML is read without DTDs or external entities.
* Files with more than 20,000 lines of coverage data show markers without per-line tooltips.
* Rider: the gutter works for editors that receive the standard editor events; C# editors are not verified.
* Stale: a file changed after the report is dimmed; Plus keeps markers on their lines with a line diff.
* CoverLens never runs tests. It only runs read-only git commands.

## FAQ

**Nothing shows in the gutter.** Check the Overview: files under "Not found" need a path mapping; the toggle "Show Coverage" may be off.
**The numbers differ from my CI.** CoverLens counts a line as covered when its hit count is above zero; JaCoCo uses covered instructions as hits.
