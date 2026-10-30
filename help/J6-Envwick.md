# Envwick help

Envwick keeps secrets out of `.env` files and out of git: it adds them to your run configurations from the system keychain. Support: atulhritik@hotmail.com

## Quick start

1. Open the **Envwick** tool window (bottom) > **Profiles** > `+` to create a profile, for example `dev`.
2. In the right-hand list press `+` to add a variable name such as `DB_PASSWORD` and type its value. The value goes to the system keychain.
3. In your run configuration, in **Environment variables**, add `ENVWICK_PROFILES=dev` (or use **Bind Run Configuration...** in the Envwick toolbar, which does this for you).
4. Run. The process starts with `DB_PASSWORD` set. In the console the value is shown as `******`.

You can also copy the line with the **Copy Binding** button under the profile list.

## How the binding works

Two marker variables in the ordinary Environment variables field of a run configuration:

| Variable | Meaning |
|---|---|
| `ENVWICK_PROFILES=dev,shared` | Add all variables of these profiles. Later profiles win over earlier ones. |
| `ENVWICK_ENV_FILE=.env` | Load this file. Plain values are used as they are; `envwick://profile/KEY` references are filled from the keychain. |

The markers hold only names, so the run configuration can be shared in the repository. Envwick removes the markers from the environment of the process. It hooks the IDE's command line layer, so it does not depend on the language: it works for any run configuration that has an Environment variables field and starts a process (Java, Kotlin, Python, Node.js, Go, PHP, Ruby, shell scripts, and so on). Values added by Envwick replace variables with the same name that are already in the environment. A path in `ENVWICK_ENV_FILE` must be inside the working directory or the project folder.

Not covered: tools that start the program through their own daemon (for example the Gradle daemon runs a task in a separate process that does not get these variables) and run configurations without an Environment variables field.

## .env files without secrets

Write secrets in the file as references:

```
DB_HOST=localhost
DB_PASSWORD=envwick://dev/DB_PASSWORD
```

The **Import .env File...** action (Pro) does this for you: choose the file, review the detected secrets (pre-selected by name and randomness; you decide), choose the profile and press **Move to Keychain**. The values are stored first; the file is rewritten only when all of them were stored. The old values are not kept anywhere. Multi-line values (private keys in quotes) are supported by the importer.

## The tool window

* **Profiles**: profiles and the variable names in them; "stored" or "not set" shows whether the keychain holds a value. Values are never displayed.
* **Usage**: which run configuration uses which secret names, and plain values left in `.env` files. Copy it as Markdown.
* **.gitignore**: files that usually hold secrets (`.env`, `.env.*` except `.env.example`, `*.pem`, `*.key`, `*.p12`, `id_rsa`, `credentials.json`, `terraform.tfvars` and similar) that git would commit. One click adds them to the root `.gitignore`. Files already tracked by git are shown as High: remove them from the index (`git rm --cached`) and replace the secrets, because they are in the history.
* **Scan**: the secret scan of the optional Envwick Plus add-on.

On a `.env` file the editor shows a banner when it is not ignored or holds plain secrets.

## Masking

Values that Envwick added are hidden in Run, Debug and Test consoles. The values are kept in memory during the IDE session for this (Settings > Tools > Envwick > Forget values held in memory). Values shorter than 4 characters are never hidden (setting). A value that the program prints in two separate pieces of console output is not recognised. Masking covers the IDE consoles only, not files or other tools your program writes to.

## Free and Pro

Free: 2 profiles with up to 8 variables each, any number of run configurations, `.env` loading, masking, usage table, `.gitignore` check. Pro (one-time purchase, 7-day trial): unlimited profiles and variables, `.env` importer. When a Pro licence ends nothing stops working; only adding beyond the free limits and importing are locked.

## Envwick Plus (optional monthly add-on, separate plugin)

Install **Envwick Plus** next to Envwick.

* **Commit scan**: in the commit window the lines you add are checked with the bundled rule pack (cloud keys, tokens, webhooks, private key blocks, JSON web tokens, passwords in connection URLs, hard-coded passwords). Findings show file, line and the first characters. Choose Commit Anyway, Review or Cancel. Strict mode (Settings > Tools > Envwick Plus) removes Commit Anyway. Switch the check off with the checkbox in the commit options.
* **Project scan** and **audit report** (Markdown, CSV, JSON; no secret values).
* **Rules**: put `envwick:allow` in a comment on a line to skip it. Settings list paths to ignore and rules to switch off. Load your own rule pack file in the Scan tab. Rule pack format, one rule per line, `#` starts a comment:

```
@version 2026.2.0
my-token|high|0|My service token|\bmst_[A-Za-z0-9]{32}\b
```

Fields: id, severity (high, medium, low), minimum entropy in bits per character (0 = no check), name, regex. A rule with a bundled id replaces the bundled rule. Rules that look like slow nested quantifiers are rejected. Nothing is downloaded; new bundled rules come with plugin updates.

## Limits and notes

* The scan finds common patterns. It cannot prove that a repository is clean. A secret that was committed once is exposed: replace it.
* The value dialog takes one line; use the importer or `.env` quoted values for multi-line secrets.
* If your system has no keychain, the IDE uses its own encrypted database and may ask for a master password.
* `.gitignore` rules are read from `.gitignore` files in the project; `.git/info/exclude` and the global ignore file are not read.
* Envwick never runs your program itself and never runs git commands. It makes no network connection.

## FAQ

**The program does not see the variable.** Check the marker spelling in Environment variables, that the profile has a value ("stored"), and that the run configuration starts the program directly. A balloon "Secret not set" appears when a value is missing.
**Can the team share profiles?** The names are in `.idea/envwick.xml`; commit it if you want. Each developer sets their own values.
**Where are the values?** In your system keychain, under entries named "IntelliJ Platform Envwick".
