---
name: supabase-keys
description: Connect Supabase with the dashboard and two keys, no MCP and no CLI. Triggers on "connect Supabase", "create the database", "table", "anon key", ".env.local", "where do I paste the SQL", RLS errors (new row violates row-level security), "data isn't saving". The user only copies from the browser and pastes into the terminal.
---

# Supabase: dashboard plus two keys

This skill governs how the workshop app connects. The official `supabase` skill covers the rest of the platform; when it suggests the MCP server or the CLI for setup, stay on this dashboard flow instead.

No MCP, no CLI. The user copies in the browser and pastes in the terminal.

## 1. Create the project (user, in the browser)

1. https://supabase.com/dashboard → **New project**
2. Name it after the app, any password (no need to save it, the app does not use it), Region **Northeast Asia (Seoul)**.
3. Provisioning takes one to two minutes. Prepare step 2 meanwhile.

## 2. Two keys into `.env.local`

Dashboard, left sidebar, **Project Settings → API**:
- **Project URL** → `NEXT_PUBLIC_SUPABASE_URL`
- **anon public** key → `NEXT_PUBLIC_SUPABASE_ANON_KEY`

In `.env.local` at the project root (copy `.env.example` if it does not exist):
```
NEXT_PUBLIC_SUPABASE_URL=https://xxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
```
Ask the user twice only: "paste the URL", then "paste the anon key". You write the file. Never echo the values back into the chat.

**Never accept the service_role key.** If they paste it by mistake (much longer, labeled `service_role`), delete it and ask for the anon key again.

After changing env vars, stop and restart `npm run dev`.

## 3. Create the table: you write the SQL, the user pastes it

Dashboard, **SQL Editor → New query**, paste, **Run**.
The starter ships `supabase/schema.sql`. It looks like this:

```sql
create table if not exists items (
  id bigint generated always as identity primary key,
  title text not null,
  done boolean not null default false,
  note text,
  created_at timestamptz not null default now()
);
alter table items enable row level security;
create policy "anyone can do anything (workshop)" on items
  for all using (true) with check (true);
```

The last two statements matter. Row Level Security stays on, but a personal workshop app gets one **allow-all** policy. Without it the app fails with `new row violates row-level security policy`. Add one line saying that a real service would add login and narrow this policy.

Changing columns works the same way: you write the `alter table` statement, the user runs it in the SQL Editor, then checks the **Table Editor**.

## 4. Using it in code (already in the starter)

```ts
import { createClient } from "@supabase/supabase-js";
export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
);
// read
const { data } = await supabase.from("items").select("*").order("created_at", { ascending: false });
// write
await supabase.from("items").insert({ title });
```

## 5. Common errors

| Symptom | Cause → fix |
|---|---|
| `Invalid API key` | anon key got truncated → paste again, restart the server |
| `relation "items" does not exist` | SQL not run yet → step 3 |
| `violates row-level security` | policy missing → rerun the last two statements of step 3 |
| Works locally, not on Vercel | env vars missing on Vercel → `vercel-deploy` skill step 3 |
| List does not update | refresh the browser. If still wrong, check the Table Editor for the row first |
