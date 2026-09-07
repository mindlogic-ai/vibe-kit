---
name: setup-machine
description: >-
  Get a non-engineer's laptop ready for building with Claude Code, from inside the chat, without an admin password.
  Triggers on "set up my machine", "check what's installed", "install git / node / gh", "준비 점검", "워크숍 준비",
  command not found for git, node, npm, npx, or gh. Mac: Claude installs Node and gh into the home folder and triggers
  the Command Line Tools popup for git. Windows: Claude checks and the user clicks through three installers.
---

# Set up the machine from chat

The user picked a folder in the Claude desktop app and is talking to you. They have no terminal habit and no admin password handy. You do the work; they click popups and log in to websites.

Always: say what you are about to install and why in one sentence, then do it. Never ask for a password. Never use `sudo`, Homebrew, or MacPorts.

## 1. Check

Run all four and report a table of ok / missing:

```bash
export PATH="$HOME/.local/bin:$PATH"
git --version; node --version; npm --version; gh --version | head -1
```

If everything is present, say so and stop. Otherwise continue with only the missing pieces.

## 2. Mac

### git (Command Line Tools)
```bash
xcode-select -p >/dev/null 2>&1 && echo "CLT present" || xcode-select --install
```
`xcode-select --install` opens a system popup. Tell the user: click **설치**, agree, wait 5 to 10 minutes. Poll with `xcode-select -p` every minute. If the user says the popup said "already installed", run `git --version` again; it may just have needed a fresh shell. Do not send the user to git-scm.com; that page starts with Homebrew.

### Node LTS, into `~/.local`, no admin
```bash
set -e
mkdir -p "$HOME/.local/bin" "$HOME/.local/opt"
ARCH=$(uname -m); [ "$ARCH" = "arm64" ] && NARCH=arm64 || NARCH=x64
NV=$(curl -fsSL https://nodejs.org/dist/index.json | python3 -c "import json,sys; print([r['version'] for r in json.load(sys.stdin) if r['lts']][0])")
curl -fsSL "https://nodejs.org/dist/$NV/node-$NV-darwin-$NARCH.tar.gz" | tar -xz -C "$HOME/.local/opt"
for b in node npm npx; do ln -sf "$HOME/.local/opt/node-$NV-darwin-$NARCH/bin/$b" "$HOME/.local/bin/$b"; done
"$HOME/.local/bin/node" --version
```

### GitHub CLI, into `~/.local`, no admin
```bash
set -e
mkdir -p "$HOME/.local/bin" "$HOME/.local/opt"
ARCH=$(uname -m); [ "$ARCH" = "arm64" ] && GARCH=arm64 || GARCH=amd64
GV=$(curl -fsSLI -o /dev/null -w '%{url_effective}' https://github.com/cli/cli/releases/latest | sed 's|.*/v||')
curl -fsSL "https://github.com/cli/cli/releases/download/v$GV/gh_${GV}_macOS_${GARCH}.zip" -o /tmp/gh.zip
unzip -qo /tmp/gh.zip -d "$HOME/.local/opt" && cp "$HOME/.local/opt/gh_${GV}_macOS_${GARCH}/bin/gh" "$HOME/.local/bin/gh" && chmod +x "$HOME/.local/bin/gh"
"$HOME/.local/bin/gh" --version | head -1
```

### PATH, once
```bash
grep -q 'HOME/.local/bin' "$HOME/.zshrc" 2>/dev/null || echo 'export PATH="$HOME/.local/bin:$PATH"' >> "$HOME/.zshrc"
```
In your own later commands, always start with `export PATH="$HOME/.local/bin:$PATH"` so they see the new tools. A terminal the user opens after this picks it up automatically.

## 3. Windows

You check; the user installs by clicking. Give the three links and the order, then re-check after each.

1. Git for Windows: https://git-scm.com/downloads/win, all defaults. After it finishes, the Claude app must be fully quit and reopened, and this conversation resumed.
2. Node LTS: https://nodejs.org, the `.msi`, all defaults.
3. GitHub CLI: in PowerShell `winget install GitHub.cli`, or the `.msi` from https://cli.github.com.

Re-check with `git --version; node --version; gh --version` in a fresh PowerShell.

## 4. GitHub login (both platforms, interactive, so the user does it)

`gh auth login` is interactive and cannot run from your Bash. Tell the user:

1. Open the app's terminal: **Ctrl+`** (Views → Terminal). On Mac make sure it is a new tab so PATH is fresh.
2. Type `gh auth login` and press Enter on every question (GitHub.com, HTTPS, Yes, Login with a web browser).
3. Copy the 8-character code, press Enter, log in on the web page that opens, paste the code.

Verify from your side:
```bash
export PATH="$HOME/.local/bin:$PATH"; gh auth status
```

## 5. Report

One table: git, node, gh, GitHub login, each with ok and the version. Then: "준비 끝. 이제 만들기 시작할 수 있어요." Hand back to the `workshop` skill.
