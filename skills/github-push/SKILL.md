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

If `gh` itself is missing, run the `setup-machine` skill (no admin password needed on Mac).

`gh auth login` is interactive: the user runs it in the app terminal (**Ctrl+`**), not you.

## 1. First upload (no repo yet)

### 1a. Starting from the workshop template, into the folder the user already opened

The user's session folder is the project. Do not create a subfolder. Clone the template in place, drop its history, then make it their own repo:
```bash
export PATH="$HOME/.local/bin:$PATH"
git clone --depth 1 https://github.com/mindlogic-ai/vibe-starter.git /tmp/vibe-starter-tpl
cp -R /tmp/vibe-starter-tpl/. . && rm -rf .git /tmp/vibe-starter-tpl
git init -b main
git add -A
git commit -m "first save"
gh repo create <repo-name> --private --source=. --push
```
If the folder already has a `.claude/` directory from the app, that is fine; it is not in the template and gets committed only if the user wants (it is harmless).

### 1b. Any other project

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
