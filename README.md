# vibe-kit

Skills for the MindLogic vibe-coding workshop (2026-09-08). Install once, then Claude Code knows the workshop flow, the prompt cards, and how to reach GitHub, Supabase, Vercel, the FactChat Gateway, and the Informe CLI with plain API keys and CLIs. No MCP servers.

## Install

Attendees use the Claude desktop app, so the kit is delivered as a zip in Slack. Unzip it and move the folders inside into `~/.claude/skills/` (Mac: Finder, Cmd+Shift+G, type the path) or `%USERPROFILE%\.claude\skills\` (Windows). Create the `skills` folder if it does not exist. Claude picks them up on the next session.

Mac one-liner, if the terminal is easier:

```bash
mkdir -p ~/.claude/skills && cp -R ~/Downloads/vibe-kit-skills/* ~/.claude/skills/
```

If Node is installed, this does the same thing:

```bash
npx skills add mindlogic-ai/vibe-kit -a claude-code -g
```

Check it worked: open any folder in the Code tab and ask "what does the workshop skill say hour one is?"

## Pre-work (20 to 30 minutes, do it before the workshop)

| Step | Mac | Windows |
|---|---|---|
| 1. Claude desktop app | download the dmg, sign in with the company account, open the Code tab | download the installer, same |
| 2. git | Terminal: `xcode-select --install`, accept the popup. Do not follow the app's git-scm.com link; that page starts with Homebrew | Install [Git for Windows](https://git-scm.com/downloads/win), all defaults, then fully restart the Claude app |
| 3. Node | [nodejs.org](https://nodejs.org) LTS installer, all defaults | same |
| 4. GitHub CLI | the macOS .pkg from [cli.github.com](https://cli.github.com), then `gh auth login` | `winget install GitHub.cli`, then `gh auth login` |
| 5. This kit | the zip from Slack, see Install above | same |

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
| `supabase` | official Supabase skill: client libraries, auth, storage, debugging errors (supabase/agent-skills) |
| `supabase-postgres-best-practices` | official Postgres rules for schema, migrations, RLS, indexes (supabase/agent-skills) |
| `vercel-react-best-practices` | official React and Next.js performance rules (vercel-labs/agent-skills) |
| `web-design-guidelines` | official UI review checklist for accessibility and UX (vercel-labs/agent-skills) |
| `impeccable` | design direction and an anti-pattern detector for "make it not look AI-generated" (Apache-2.0, pbakaus) |
| `make-interfaces-feel-better` | polish details: spacing, shadows, typography, motion (jakubkrehel) |

## When you outgrow this kit

```
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills      # Word, PowerPoint, Excel, PDF output
/plugin install frontend-design@claude-plugins-official      # stronger UI generation
```

Each installed skill adds its description to every session, so add these when you need them.

The four official skills bundled here (supabase, supabase-postgres-best-practices, vercel-react-best-practices, web-design-guidelines) are verbatim copies from `supabase/agent-skills` and `vercel-labs/agent-skills` as of 2026-09-07. To refresh them: `npx skills add supabase/agent-skills -a claude-code -g` and `npx skills add vercel-labs/agent-skills -a claude-code -g`.

## Starter app

The hour-one build starts from [mindlogic-ai/vibe-starter](https://github.com/mindlogic-ai/vibe-starter): Next.js, Supabase client, one board page, the SQL for its table, and a `CLAUDE.md` that tells Claude the rules.

## Note on impeccable

`impeccable` ships as a launcher. The first time Claude runs `impeccable detect` or `impeccable context`, the launcher downloads a 16 MB engine binary from pbakaus/impeccable's GitHub releases into `~/.impeccable/`. That is expected; it only happens once.
