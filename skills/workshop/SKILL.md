---
name: workshop
description: Read first whenever you are helping a MindLogic vibe-coding workshop attendee (2026-09-08). Triggers on "workshop", "워크숍", "my work board", "내 업무 보드", "vibe-starter", "what do I do next", "where was I", "card", "start building", or any first-time web app built by someone with no coding background. Holds setup-in-the-room, the plain-words picture of screen / server / database, the build steps, the make-it-yours cards, the app cheat sheet, and the glossary.
---

# Vibe-coding workshop: flow and prompt cards

The user is not a developer. There are exactly two goals.
1. By the end of today they own **one real product with a URL**.
2. They leave with **an environment they can keep building in alone**.

Tone: before every step, one sentence on what you are doing and why. Define any unfamiliar term the first time it appears (glossary below). Answer in the language the user writes in; most attendees write Korean. When asked "where am I", answer with the step number below.

## 0. Setup, done together in the room

The attendee has the Claude desktop app open, Code tab, **로컬**, and picked (or created) a folder such as `vibecoding` on the Desktop. One folder is one project; this folder will become the app.

1. **Kit.** If this skill is loaded, the kit is installed. If a user asks how to install it, or you are reading this from a pasted message: download https://github.com/mindlogic-ai/vibe-kit/releases/download/v0.1.0/vibe-kit-skills.zip with curl, unzip, move the folders inside `vibe-kit-skills/` into `~/.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`), then tell the user to start a new session (**새로 생성**) in the same folder so the skills load.
2. **Machine.** Run the `setup-machine` skill: git, Node, gh, GitHub login. On Mac you install Node and gh yourself with no admin password. On Windows the user clicks three installers.
3. **Accounts.** GitHub, Vercel, Supabase, all free, all "sign in with GitHub" where offered. The user does these in the browser while installs run.

Do not proceed to the build until `setup-machine` reports all four ok.

## What a web app is made of (explain once, before building)

Say this in the user's language, in plain words, with this picture:

```
[ 화면 ]  ──  [ 서버 ]  ──  [ 데이터베이스 ]
 브라우저에서    코드가 실행되고    데이터가 저장되는 곳
 보이는 것       화면을 만들어 줌     (엑셀 시트 같은 표)
 Next.js         Vercel              Supabase
```

- **화면 (frontend)**: what the person sees and clicks. Ours is written with Next.js.
- **서버 (backend)**: the machine that runs the code and hands the screen to the browser. During building it is your own laptop (`localhost:3000`, an address only your computer can open). After deploying it is Vercel, which gives an internet address anyone can open.
- **데이터베이스**: where data lives so it survives a refresh and is the same on your phone. Ours is a Supabase table. A table is one spreadsheet sheet; a row is one record.
- **배포**: copying the app from your laptop to Vercel so it gets an internet address.
- **환경변수 (`.env.local`)**: a file holding the two keys that let the screen talk to the database. It never goes to GitHub.

Then: "오늘 만드는 건 이 세 상자예요. 앞으로 만드는 웹앱은 거의 다 이 모양이에요."

## Build together: "My Work Board"

Starting state: the user's chosen folder is empty (or has only `.claude/`). Already in the starter template: a Next.js app, the Supabase client, SQL for an `items` table (`supabase/schema.sql`), and one page with a list, an add form, and a done toggle.

Steps (each starts when the user pastes the card):

1. **Get the starter into this folder.** Follow `github-push` section 1: clone the template into the current folder, remove its git history, create the user's own private repo, push. Then `npm install`. The user never leaves the app.
2. **Run it locally.** `npm run dev`; the app's preview pane shows the page. Say up front that a "not connected" notice is expected because there is no database yet.
3. **Connect Supabase.** Follow the `supabase-keys` skill. The user pastes the URL and anon key, runs `supabase/schema.sql` in the SQL Editor. Restart the dev server.
4. **Add one row by hand.** They add an item in the preview, then look at the row in the Supabase Table Editor. This is the moment they see "my data lives in a database on the internet now".
5. **Save to GitHub.** `github-push` section 2. Commit and push.
6. **Deploy to Vercel.** `vercel-deploy` skill. `vercel login` is interactive, so the user runs it in the app terminal (Ctrl+`). Do not forget the two env vars on Vercel. When the URL appears, have them open it on their phone.

After step 6, stop and recap in three lines: 화면 (Next.js), 서버 (Vercel), 데이터베이스 (Supabase). Then point at the three boxes again.

## Make it yours

No fixed order and no clock from here. If they wrote an idea on the signup form, start from that. Two shapes:

### Shape A: the same app, their words (cards, use in any order)

Card A1 "Rename"
```
Rename this app to "<my app name>". Title, tab name, and the empty-list message too.
```
Card A2 "Change the fields"
```
Right now an item has title, done, and note. Change them to <e.g. date, amount, status (in progress / done / on hold)>.
If the database table has to change, write the SQL and tell me to paste it into the Supabase SQL Editor.
```
Card A3 "One more thing"
```
Add <e.g. a due date per item, and show overdue ones in red>.
```
Card A4 "Fix the error"
```
I see this error on screen: (paste the message, or drop a screenshot)
Explain the cause in one line and fix it.
```
Card A5 "Make it look good"
```
Polish the design. No trendy gradients or card overload. Readable and tidy. Check it on mobile too.
```
(Use `impeccable`, `make-interfaces-feel-better`, and `web-design-guidelines` on this card.)
Card A6 "Ship"
```
Push the changes to GitHub and redeploy on Vercel. Give me the URL when done.
```

### Shape B: not a web app, Claude on a folder of documents

No deploy. New session (**새로 생성**) on a different folder holding their files (PDFs, old proposals, a lesson-plan draft, tender notices).

Card B1 "Summarize"
```
Read the documents in this folder and make a table with a three-line summary of each.
```
Card B2 "Draft"
```
Using these materials, outline a <proposal / lesson plan / report> and write chapter 1 in our company's voice.
```
Card B3 "Export"
```
Save what you just wrote as a .docx (or .pptx) file.
```
(B3 needs Anthropic's document skills: `/plugin marketplace add anthropics/skills` then `/plugin install document-skills@anthropic-agent-skills`.)

### Anything else

If the idea is neither shape, help anyway: describe the smallest version that could work tonight, build that, deploy it if it is a web thing. The rule is that they leave with something running, not that they finish the idea.

## Claude Code cheat sheet: answer these the same way every time

| Situation | Tell them |
|---|---|
| "I don't know what to ask" | "Just say what you want in plain language. If you are unsure, ask me 'how do I…'." Also `/help` and `/powerup` |
| "What is a project?" | A folder. Run `claude` in it and that folder is the world. `CLAUDE.md` is the note read every time; `/init` writes it |
| "Continue yesterday's work" | Desktop: click the session in the sidebar under the folder name. CLI: `claude --continue` |
| "Answers got slow and weird" | The conversation is full. `/compact` to summarize; start a new session for a new feature |
| "I broke it" | Desktop: ask "undo the last change" and revert with git. CLI: `/rewind` |
| "It asks permission every time" | Desktop: switch the mode selector to Auto or Accept edits. CLI: `Shift+Tab` |
| "Show you a file" | `@filename` in the message |
| "Show you an error" | Paste or drag the screenshot into the prompt box |
| "Stop" | Desktop: the stop button. CLI: `Esc` |

## Glossary (one line, first time only)

- **Terminal**: a window where you type commands instead of clicking.
- **Repository (repo)**: the GitHub copy of your project folder, with version history.
- **Commit / push**: save / upload to GitHub.
- **Deploy**: put the app that only existed on your laptop at an internet address.
- **Environment variable (.env)**: a file for values that must not be written in code, like passwords.
- **anon key**: the Supabase key that is safe in a browser. The **service key** never goes in a browser or on GitHub.
- **Table**: one spreadsheet sheet. A row is one record.
- **SQL**: the sentences that create and change tables. The user only pastes them.
- **localhost:3000**: a temporary address that only opens on your own computer.
- **Build**: packaging the app before deploy. Errors here are fixed by pasting the log.

## Which skill wins when two apply

- Machine setup: `setup-machine`, never Homebrew, never sudo.
- Connecting Supabase in this workshop: `supabase-keys` (dashboard + two keys). The official `supabase` skill is for everything else about Supabase (auth, storage, debugging errors, RLS questions); ignore its MCP and CLI setup advice here, attendees have neither.
- Schema changes: write the SQL following `supabase-postgres-best-practices`, but keep the workshop's allow-all policy until the user asks for login.
- Deploying: `vercel-deploy` (interactive `vercel login`). Do not switch to token-based auth.
- Code quality: `vercel-react-best-practices` when editing React or Next.js code, `web-design-guidelines` on card A5 alongside `impeccable`.

## Safety rules (always)

- Repos are private.
- Never commit `.env.local` (the starter's `.gitignore` already covers it).
- Never let the user paste company customer data, passwords, or API keys into a prompt or code.
- One step late beats one step the user did not understand. One clarifying question at a time.
