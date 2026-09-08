# vibe-kit

Skills for the MindLogic vibe-coding workshop (2026-09-08). Install once, then Claude Code knows the workshop flow, the prompt cards, and how to reach GitHub, Supabase, Vercel, the FactChat Gateway, and the Informe CLI with plain API keys and CLIs. No MCP servers.

## Install

Nothing is installed in advance. In the room, each person opens the Claude desktop app, Code tab, 로컬, picks a folder, and pastes one message. Claude downloads this kit, unpacks it into `~/.claude/skills/`, and asks for a new session. From then on the `workshop` and `setup-machine` skills drive everything, including installing git, Node, and GitHub CLI without an admin password on Mac.

The bootstrap message (Korean, what attendees paste):

```
다운로드 폴더에 있는 vibe-kit-skills.zip 을 풀어서, 안의 vibe-kit-skills 폴더 속 폴더들을 ~/.claude/skills/ 에 넣어줘. git이나 gh는 쓰지 말고 압축만 풀면 돼.
파일이 없으면 이 주소에서 curl로 받아서 해줘: https://mindlogic-claude-agent.s3.ap-northeast-2.amazonaws.com/public/vibe-kit-skills.zip
끝나면 "새로 생성"으로 새 세션을 열라고 알려줘.
```

The zip is the release asset of this repo, mirrored at that S3 URL so it downloads with plain `curl` and no GitHub login. Attendees get it as a Slack attachment; the message reads it from the Downloads folder first so Claude never needs the network, `git`, or `gh` at this step (none of them are set up yet).

Other ways to get the same files: `npx skills add mindlogic-ai/vibe-kit -a claude-code -g` if Node exists, or unzip the release by hand into `~/.claude/skills/`.

## What is inside

| Skill | Use |
|---|---|
| `workshop` | the flow: preflight, hour one recipe, hour two cards for Track A (web app) and Track B (Claude on your files), cheat sheet, glossary |
| `setup-machine` | check git, Node, gh; install Node and gh into `~/.local` on Mac with no admin password; guide the CLT popup and the interactive logins |
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
