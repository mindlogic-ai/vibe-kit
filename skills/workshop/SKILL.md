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

1. **Kit.** If this skill is loaded, the kit is installed. If a user asks how to install it, or you are reading this from a pasted message: the zip `vibe-kit-skills.zip` is normally already in the user's Downloads folder (they saved it from Slack). Unzip it and move the folders inside `vibe-kit-skills/` into `~/.claude/skills/` (Windows: `%USERPROFILE%\.claude\skills\`). If the file is not there, fetch it with plain `curl -L` from https://mindlogic-claude-agent.s3.ap-northeast-2.amazonaws.com/public/vibe-kit-skills.zip (no login needed). Never use `git` or `gh` for this step; they are not installed yet. Then tell the user to start a new session (**새로 생성**) in the same folder so the skills load.
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
- **서버 (backend)**: the machine that runs the app code, hands the screen to the browser, and does anything that needs a secret key (an AI call, for example). During building it is your own laptop (`localhost:3000`, an address only your computer can open). After deploying it is Vercel, which gives an internet address anyone can open. In this board the screen saves to Supabase directly, so the server box only serves the page until AI is added. If someone says "isn't Supabase the backend?", say they are half right: Supabase also carries the save/read API, which is why the screen can talk to it directly.
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

**A good first message says four things.** Coach the user toward this, in their language, without asking for code: (1) who uses it and for what, one sentence; (2) the three or four fields the screen shows, as words; (3) what is out for tonight (login, payment, notifications); (4) how they will check it ("tell me what to click on localhost:3000 when done"). If a message is missing one of these, ask for that one thing only, then build.

**Signup ideas and the smallest version to build tonight** (the guide shows the same list with copy-paste messages):

| Idea | Shape | Tonight's smallest version |
|---|---|---|
| to-do app | A | due date + priority, overdue rows first and red, "today only" filter |
| personal work dashboard | A | three tabs (in progress / due this week / done), counts on top |
| life or cycle tracker | A | date, kind, 1-5 score, memo; 30-day line chart; one-button "log today" |
| child growth planner | A | date, height, weight, one line; height/weight chart; photos later |
| shopping mall | A | products (name, price, image url, stock), detail page, cart count; `/admin` add-product form; no payment |
| monthly finance data | B | read the Excel files in the folder, monthly totals by category, flag >20% month-over-month changes, save a new Excel |
| training material (교안) | B | table of contents first, wait for OK, then per-session goals and exercises |
| PPT proposal | B | 12-slide outline (title + three key messages each), then `.pptx` via document-skills |
| tender summary + alerts | B first | table per notice: name, budget, deadline, fit (yes/maybe/no) + one-line why, sorted by deadline; automation is the next step |
| no idea yet | A or B | Interview them: one question at a time, five at most, aimed at what repeats in their work (what they track in Excel or memos today, what they copy between tools, what they report weekly). Then propose three ideas that can run tonight, each as three lines of what the screen shows. They pick, you build. Do not offer a generic list. |

When the user is stuck on what to ask next, offer this: summarize what exists in three lines and propose three next steps, let them pick.

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
Card A7 "Login"
```
Add login: Supabase Auth, email + password. Only the signed-in user's rows are visible; change the RLS policy to match. Tell me one step at a time what to switch on or paste in the Supabase dashboard.
```
(Only when they want to share the URL with others. Use the official `supabase` skill for Auth; replace the allow-all policy with per-user policies; disable email confirmation in Authentication → Providers → Email for tonight or they will wait for a mail.)
Card A8 "Add AI"
```
Add an "AI summary" button per item. Call the company FactChat gateway, keep the key server-side only, open .env.local for me and I will paste the key. Put the key on Vercel too when done.
```
(Use `factchat-gateway`. The key comes from factchat-cloud.mindlogic.ai → admin → API keys → issue, shown once. Open `.env.local` in the file pane for them to paste; never ask them to paste the key into the chat. If they have no issue menu, the tenant is not enabled: send them to Jaeho.)

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

**Start over when fixing is slower than rebuilding.** If a session has piled up three or more failed fixes, say so and offer: new folder, new session, one first message that states everything they now know they want. The second attempt is usually better than the patched first one.

**Missing capability: find a skill.** When they ask for something no installed skill covers (image generation with GPT Image 2, PDF parsing, a specific design system), search skills.sh (`npx skills find <term>` or the site) and install the best match into `~/.claude/skills/` with `npx skills add <owner/repo> --skill <name> -a claude-code -g`, then tell them to open a new session. Say what you installed and where it came from.

## Claude Code cheat sheet: answer these the same way every time

| Situation | Tell them |
|---|---|
| "I don't know what to ask" | "Just say what you want in plain language. If you are unsure, ask me 'how do I…'." Also `/help` and `/powerup` |
| "What is a project?" | A folder. Run `claude` in it and that folder is the world. `CLAUDE.md` is the note read every time; `/init` writes it |
| "Continue yesterday's work" | Desktop: click the session in the sidebar under the folder name. CLI: `claude --continue` |
| "Answers got slow and weird" | The conversation is full. `/compact` to summarize; start a new session for a new feature |
| "I broke it" | Desktop: ask "undo the last change" and revert with git. CLI: `/rewind` |
| "It asks permission every time" | Desktop: switch the mode selector next to the send button to **Auto** (the workshop default). Accept edits if Auto is missing. CLI: `Shift+Tab` |
| "Which model?" | Leave the dropdown next to the send button on its default (Fable 5.1). No reason to change tonight |
| "Where do I see the app?" | The desktop app's Browser pane (Cmd/Ctrl+Shift+B). You start the dev server and it opens there; you can screenshot, click, and verify your own changes in it. They can click around in it too |
| "I want to change this thing on screen" | In the Browser pane, Cmd/Ctrl+Shift+S selects an element to point at; or paste a screenshot; or describe it in words. All three work |
| "How much have I used?" | The usage ring next to the model picker shows this session's context and the week's plan usage. Company Max plan: tell them to use the weekly quota fully, it resets |
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
