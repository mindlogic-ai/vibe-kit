---
name: vercel-deploy
description: First or repeat deploy to Vercel. Triggers on "deploy", "give it an internet address", "Vercel", "build failed", "deployed but the page is blank / errors", adding env vars on Vercel, auto-deploy on push. Uses the Vercel CLI and dashboard only, no MCP.
---

# Vercel deploy: CLI once, then just push

## 0. Preconditions
- A GitHub repo exists (`github-push` skill). If not, do that first.
- `npm run build` passes locally. Always run it once before deploying. An error here is an error on Vercel.

## 1. Login (once)

```bash
npm i -g vercel
vercel login
```
The browser opens; log in with the GitHub account. If Windows cannot find `vercel`, open a new terminal.

## 2. First deploy

In the project folder:
```bash
vercel
```
Press Enter on every question (defaults). It prints a `Preview:` URL. That is a preview. The real one:
```bash
vercel --prod
```
Give the user the `https://<name>.vercel.app` after `Production:`. Have them open it on their phone.

## 3. Env vars (the Supabase keys). Skipping this gives a blank page or errors.

```bash
vercel env add NEXT_PUBLIC_SUPABASE_URL production
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY production
```
Each prompts for the value, same as in `.env.local`. Then `vercel --prod` again.

Dashboard route: https://vercel.com → project → **Settings → Environment Variables**.

## 4. Auto-deploy on push (the "keep building alone" part)

https://vercel.com/new → **Import Git Repository** → pick the repo → Deploy.
From then on `git push` alone publishes a new version in one or two minutes. Step 2's CLI deploy is day-one only; after this the CLI is not needed.

If the project was already created by the CLI: dashboard → project → **Settings → Git → Connect Git Repository**.

## 5. When the build fails

```bash
vercel logs <deployment url>
```
Or dashboard → Deployments → the failed one → **Build Logs**. Have the user paste the last 20 lines (or Ctrl+V a screenshot). Cause in one line → fix → `npm run build` locally → push.

| Symptom | Cause → fix |
|---|---|
| `Type error` / `Module not found` | local build fails too → fix locally, push |
| Deploy succeeded, page blank | env vars missing → step 3 |
| `Invalid API key` | truncated anon key on Vercel → step 3 again |
| 404 | wrong URL. `vercel ls` shows the production one |

## Do not
- Commit `.env.local` to get values onto Vercel. Env vars go in through step 3 only.
- Suggest buying a domain. `*.vercel.app` is enough.
