---
name: github-push
description: First-time GitHub upload. Triggers on "put it on GitHub", "create a repo", "push", gh login, "git push fails", auth errors (403, Permission denied, could not read Username). One `gh` login, no GitHub Desktop.
---

# Save to GitHub with gh

## 0. Check login

```bash
gh auth status
```
If it shows `Logged in to github.com account <id>`, go to step 1. Otherwise:

```bash
gh auth login
```
Answers: `GitHub.com` → `HTTPS` → `Yes` (let gh handle git auth) → `Login with a web browser`. Copy the 8-character code from the terminal into the browser. This one login also makes `git push` work without a password from now on.

If `gh` itself is missing: Mac uses the macOS .pkg from https://cli.github.com, Windows runs `winget install GitHub.cli` in PowerShell. Open a new terminal afterwards.

## 1. First upload (no repo yet)

Inside the project folder:
```bash
git init -b main 2>/dev/null || true
git add -A
git commit -m "first save"
gh repo create <repo-name> --private --source=. --push
```
`<repo-name>` is the app name in lowercase letters and hyphens. Always `--private`.

Then `gh repo view --web` to show it in the browser.

## 2. Every later change

```bash
git add -A
git commit -m "<one line on what changed>"
git push
```

## 3. Common errors

| Symptom | Cause → fix |
|---|---|
| `could not read Username` / `Permission denied` | not logged in → `gh auth login` again |
| `Please tell me who you are` | no git identity → `git config --global user.name "Name"`, `git config --global user.email "work email"` |
| `.env.local` got pushed | rotate the keys now. `git rm --cached .env.local`, commit, confirm `.env*` is in `.gitignore` |
| Windows cannot find `git` | install Git for Windows, restart the terminal |

## Do not
- Create public repos.
- Make the user generate SSH keys. gh over HTTPS is enough.
