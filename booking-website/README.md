# AI Booking Assistant — Demo Website

A real, working AI booking chatbot for clinics, dental practices, and salons.
Chat is powered by Google Gemini (via a serverless backend that keeps your API
key private). Confirmed bookings are sent to your n8n webhook, which writes
them to your Google Sheet and emails you.

## Files

- `index.html` — the frontend (chat UI, no build step needed)
- `api/chat.js` — Vercel serverless function that calls Gemini on the server
  side, so your API key is never exposed in the browser
- `package.json` — minimal project file, no dependencies required

## 1. Get a Gemini API key

1. Go to https://aistudio.google.com/apikey
2. Create an API key (free tier is fine to start)
3. Keep it somewhere safe — you'll paste it into Vercel, never into the code

## 2. Push this folder to GitHub

```
cd booking-website
git init
git add .
git commit -m "Initial commit"
```

Create a new repo on GitHub, then:

```
git remote add origin <your-repo-url>
git branch -M main
git push -u origin main
```

## 3. Deploy on Vercel

1. Go to https://vercel.com and sign in (GitHub login is easiest)
2. Click "Add New Project" and import this GitHub repo
3. Before deploying, add an Environment Variable:
   - Name: `GEMINI_API_KEY`
   - Value: (paste your Gemini API key from step 1)
4. Click Deploy

Vercel will give you a live URL like `your-project.vercel.app` — that's the
link you send to prospects.

## 4. Point it at your n8n webhook

In `index.html`, near the top of the `<script>` block, there's:

```js
const N8N_WEBHOOK_URL = 'https://basicdigi.app.n8n.cloud/webhook/new-booking';
```

Update this to your actual production webhook URL if it changes, commit, and
push — Vercel redeploys automatically on every push to `main`.

## Notes

- If chat requests fail, check your Vercel project's Function Logs for the
  actual Gemini API error — usually a missing/invalid `GEMINI_API_KEY`.
- The Gemini model used is `gemini-2.5-flash` (set in `api/chat.js`) — cheap
  and fast, good fit for this use case. You can change it there if needed.
- Since this is a normal website (not a sandboxed environment), the webhook
  call to n8n works with no restrictions, as long as your n8n webhook's
  "Allowed Origins (CORS)" setting is `*` (the default) or includes your
  Vercel domain.
