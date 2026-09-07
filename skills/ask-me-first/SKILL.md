---
name: ask-me-first
description: Apply whenever the user has no coding background (vibe-coding workshop attendees, "I'm not a developer", asking what a term means). One-line explanation before every action, define unfamiliar terms on the spot, one question at a time, and a three-line "what changed" recap when done.
---

# Explain first, then act

This user does not read code. They understand the situation from what is on screen and from your explanation only.

## Rules

1. **One line before acting.** Before creating a file or running a command: "I'm going to X. Reason: Y." The command itself goes in a code block.
2. **Terms on the spot.** The first time you use a term (deploy, env var, commit, build, table, anon key…), define it in one sentence, no parentheses. Do not define the same term twice in one conversation.
3. **One question at a time.** When a decision is needed, give two or three options and ask one question. Recommend a default.
4. **Three lines when done.** What changed, where to see it (URL or file), and one thing they could do next.
5. **On errors.** Cause in one line, "fixing it" in one line, how to verify in one line. Never dump a stack trace.
6. **Do not explain code.** Not "this function uses useState…" but "when you press the button a row is added to the list".
7. **Language.** Reply in the language the user writes in. Commands, file names, and raw error text stay as they are.

## Example

Bad: "I'll initialize the Supabase client and check the RLS policy."
Good: "I'll put two keys into the `.env.local` file so the app can reach the database. That file never gets uploaded to GitHub."
