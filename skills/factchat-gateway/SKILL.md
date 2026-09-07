---
name: factchat-gateway
description: Add AI to the user's app. Triggers on "AI summarize button", "add a chatbot", "generate an image", "TTS", "FactChat API", "gateway", "where do I get an API key", swapping the OpenAI SDK base_url. MindLogic's FactChat Gateway is OpenAI-compatible and one key covers LLM, image, audio, and video models. Shows how to keep the key out of the browser.
---

# FactChat Gateway: adding AI to your app

One API key calls models from several vendors (Claude, GPT, Gemini, …) in the same format. It is OpenAI-compatible, so only `base_url` changes.

```
BASE = https://factchat-cloud.mindlogic.ai/v1/gateway
Auth = Authorization: Bearer <API_KEY>
```

## 1. Get a key (user, in the browser)

Log in to FactChat (https://factchat-cloud.mindlogic.ai) → admin → **API keys** → issue one. It is shown once. Tell the user to put it in `.env.local`; never echo the value into the chat.

```
FACTCHAT_API_KEY=fc_...
```
Do **not** prefix it with `NEXT_PUBLIC_`. That would expose it to the browser.

A 403 means the gateway is off for the organization → ask Jaeho. A 404 `Model not found` means a free-tier model was requested → pick one from step 2.

## 2. Which models can be called

```bash
curl -s "$BASE/models/?type=llm" -H "Authorization: Bearer $FACTCHAT_API_KEY" | jq -r '.data[].id'
```
Only what this lists is callable. Workshop defaults: text `claude-sonnet-5` (fast, cheap), images `gemini-3.1-flash-lite-image`.

## 3. The standard Next.js shape: key stays on the server

Never call the gateway from a client component. Add one **Route Handler** and let the browser call that.

`app/api/ai/route.ts`
```ts
import OpenAI from "openai";
export async function POST(req: Request) {
  const { text } = await req.json();
  const client = new OpenAI({
    apiKey: process.env.FACTCHAT_API_KEY,
    baseURL: "https://factchat-cloud.mindlogic.ai/v1/gateway",
  });
  const r = await client.chat.completions.create({
    model: "claude-sonnet-5",
    messages: [{ role: "user", content: `Summarize the following in three lines:\n${text}` }],
  });
  return Response.json({ result: r.choices[0].message.content });
}
```
Install with `npm i openai`. Browser side: `fetch("/api/ai", { method: "POST", body: JSON.stringify({ text }) })`.

Put the same key on Vercel: `vercel env add FACTCHAT_API_KEY production`.

## 4. Other lanes (only when asked)

| Want | Endpoint | Note |
|---|---|---|
| Image generation | `POST $BASE/images/generations/` | `{"model":"gemini-3.1-flash-lite-image","prompt":"..."}` |
| Speech (TTS) | `POST $BASE/audio/speech/` | response is raw PCM 24kHz; ffmpeg to mp3 |
| Anthropic native | `$BASE/claude/v1/messages/` | base_url for the Anthropic SDK |
| A whole Studio chatbot | `$BASE/chatbots/{id}/chat/completions/` | its documents, tools, persona included. `model` is ignored |

Billing is FactChat credits. The response's `credits` field is this call's cost.

## Do not
- Make the key `NEXT_PUBLIC_`, call from a client component, or push it to GitHub.
- Call free-tier models (`gpt-5-nano` and the like). Step 2's list is the truth.
