# vibe-kit

Skills for the MindLogic vibe-coding workshop (2026-09-08). Install once, then Claude Code knows the workshop flow, the prompt cards, and how to reach GitHub, Supabase, Vercel, the FactChat Gateway, and the Informe CLI with plain API keys and CLIs. No MCP servers.

## Install (one line, after Node is installed)

```bash
npx skills add mindlogic-ai/vibe-kit -a claude-code -g
```

Or from inside Claude Code:

```
/plugin marketplace add mindlogic-ai/vibe-kit
/plugin install vibe-kit@vibe-kit
```

No Node yet, or `npx` fails on the office Wi-Fi: download the zip from the Releases page and unzip it so the folders land in `~/.claude/skills/` (Mac) or `%USERPROFILE%\.claude\skills\` (Windows).

Check it worked: start `claude` anywhere and ask "what does the workshop skill say hour one is?"

## Pre-work (20 to 30 minutes, do it before the workshop)

| Step | Mac | Windows |
|---|---|---|
| 1. Open a terminal | Cmd+Space, type Terminal | Win+X, choose Terminal or PowerShell |
| 2. git | Run `git --version`; accept the Command Line Tools popup and wait | Install [Git for Windows](https://git-scm.com/downloads/win), all defaults |
| 3. Claude Code | `curl -fsSL https://claude.ai/install.sh \| bash` | `irm https://claude.ai/install.ps1 \| iex` |
| 4. Log in | Type `claude`, the browser opens, use the company Claude account | same |
| 5. Node | [nodejs.org](https://nodejs.org) LTS installer, all defaults | same |
| 6. GitHub CLI | the macOS .pkg from [cli.github.com](https://cli.github.com), then `gh auth login` | `winget install GitHub.cli`, then `gh auth login` |
| 7. This kit | the install line above | same |

Accounts to have (all free): [GitHub](https://github.com/signup), [Vercel](https://vercel.com/signup), [Supabase](https://supabase.com/dashboard/sign-up).

## What is inside

| Skill | Use |
|---|---|
| `workshop` | the flow: preflight, hour one recipe, hour two cards for Track A (web app) and Track B (Claude on your files), cheat sheet, glossary |
| `ask-me-first` | explain before acting, define terms, one question at a time |
| `github-push` | `gh auth login`, create a private repo, push |
| `supabase-keys` | project, URL and anon key into `.env.local`, SQL pasted into the SQL Editor |
| `vercel-deploy` | Vercel CLI login and deploy, env vars, auto-deploy on push, reading build logs |
| `factchat-gateway` | add AI to the app through MindLogic's OpenAI-compatible gateway, key kept server-side |
| `informe-cli` | install `inf` (uv, cloudflared) and talk to the company agent |
| `impeccable` | design direction and an anti-pattern detector for "make it not look AI-generated" (Apache-2.0, pbakaus) |
| `make-interfaces-feel-better` | polish details: spacing, shadows, typography, motion (jakubkrehel) |

## When you outgrow this kit

```
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills      # Word, PowerPoint, Excel, PDF output
/plugin install frontend-design@claude-plugins-official      # stronger UI generation
npx skills add vercel-labs/agent-skills --skill web-design-guidelines -a claude-code -g
npx skills add vercel-labs/agent-skills --skill react-best-practices -a claude-code -g
npx skills add supabase/agent-skills --skill supabase-postgres-best-practices -a claude-code -g
```

Each installed skill adds its description to every session, so add these when you need them, not all at once.

## Starter app

The hour-one build starts from [mindlogic-ai/vibe-starter](https://github.com/mindlogic-ai/vibe-starter): Next.js, Supabase client, one board page, the SQL for its table, and a `CLAUDE.md` that tells Claude the rules.

## Note on impeccable

`impeccable` ships as a launcher. The first time Claude runs `impeccable detect` or `impeccable context`, the launcher downloads a 16 MB engine binary from pbakaus/impeccable's GitHub releases into `~/.impeccable/`. That is expected; it only happens once.
