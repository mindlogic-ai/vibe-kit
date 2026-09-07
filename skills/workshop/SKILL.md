---
name: workshop
description: Read first whenever you are helping a MindLogic vibe-coding workshop attendee (2026-09-08). Triggers on "workshop", "my work board", "vibe-starter", "what do I do next", "where was I", "hour one / hour two", "card", or any first-time web app built by someone with no coding background. Holds the goals, the step order, the copy-paste prompt cards, the Claude Code cheat sheet, and the glossary.
---

# Vibe-coding workshop: flow and prompt cards

The user is not a developer. There are exactly two goals.
1. By the end of today they own **one real product with a URL**.
2. They leave with **an environment they can keep building in alone**.

Tone: before every step, one sentence on what you are doing and why. Define any unfamiliar term the first time it appears (glossary below). Answer in the language the user writes in; most attendees write Korean. When asked "where am I", answer with the step number below.

## 0. Preflight (run in order when asked)

| # | Command | Healthy output |
|---|---|---|
| 1 | `git --version` | `git version 2.x` |
| 2 | `claude --version` | `2.1.xxx (Claude Code)` |
| 3 | `node --version` | `v22` or higher |
| 4 | `gh auth status` | `Logged in to github.com` |

If any fails, follow the matching skill (`github-push`) or the README's pre-work section.

## Hour one: everyone builds the same thing, "My Work Board"

Starting state: the user ran `claude` inside the `vibe-starter` folder.
Already in the starter: a Next.js app, the Supabase client, SQL for an `items` table (`supabase/schema.sql`), and one page with a list, an add form, and a done toggle.

Steps (each starts when the user pastes the card):

1. **Run it locally.** Start `npm run dev` and have them open `http://localhost:3000`. Say up front that an empty list or an error is expected because there is no database yet.
2. **Connect Supabase.** Follow the `supabase-keys` skill. The user pastes the URL and anon key into `.env.local` and runs `supabase/schema.sql` in the SQL Editor. Restart the dev server.
3. **Add one row by hand.** They add an item in the browser, then look at the row in the Supabase Table Editor. This is the moment they see "my data lives in a database on the internet now".
4. **Save to GitHub.** `github-push` skill. Private repo.
5. **Deploy to Vercel.** `vercel-deploy` skill. Do not forget the two env vars on Vercel. When the URL appears, have them open it on their phone.

After step 5 hour one is done. Stop and recap in three lines: screen (Next.js), storage (Supabase), address (Vercel).

## Hour two: make it theirs

Two tracks, the user picks. If they wrote an idea on the signup form, start from that.

### Track A: turn the web app into their own domain (cards in order)

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
I see this error on screen: (paste the message, or Ctrl+V a screenshot)
Explain the cause in one line and fix it.
```
Card A5 "Make it look good"
```
Polish the design. No trendy gradients or card overload. Readable and tidy. Check it on mobile too.
```
(Use the `impeccable` and `make-interfaces-feel-better` skills on this card.)
Card A6 "Ship"
```
Push the changes to GitHub and redeploy on Vercel. Give me the URL when done.
```

### Track B: not a web app, put Claude to work on my files

No deploy. Make a folder, drop files in it (PDFs, old proposals, a lesson-plan draft, tender notices), run `claude` in that folder.

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

## Claude Code cheat sheet: answer these the same way every time

| Situation | Tell them |
|---|---|
| "I don't know what to ask" | "Just say what you want in plain language. If you are unsure, ask me 'how do I…'." Also `/help` and `/powerup` |
| "What is a project?" | A folder. Run `claude` in it and that folder is the world. `CLAUDE.md` is the note read every time; `/init` writes it |
| "Continue yesterday's work" | Same folder, `claude --continue`, or `/resume` |
| "Answers got slow and weird" | The conversation is full. `/context` to check, `/compact` to summarize, `/clear` and start fresh for a new feature |
| "I broke it" | `/rewind` rolls back both the chat and the files |
| "It asks permission every time" | `Shift+Tab` to accept-edits mode. Risky commands still ask |
| "Show you a file" | `@filename` in the message |
| "Show you an error" | Copy a screenshot, then `Ctrl+V` (Ctrl on Mac too) |
| "Stop" | `Esc` |
| "New line" | `\` then Enter, or Shift+Enter |
| "How much have I used" | `/usage` |

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

## Safety rules (always)

- Repos are private.
- Never commit `.env.local` (the starter's `.gitignore` already covers it).
- Never let the user paste company customer data, passwords, or API keys into a prompt or code.
- One step late beats one step the user did not understand. One clarifying question at a time.
