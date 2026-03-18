# PhoneTrace — Setup Guide

## Why You Got "Trace Failed"

Browsers block direct calls to `api.anthropic.com` for two reasons:
1. **CORS** — Anthropic's API doesn't allow browser requests (security policy)
2. **No API key** — the key can't live in client-side code where anyone can steal it

The fix: a **Cloudflare Worker** that sits in the middle, adds your key, and forwards the request.
It's free and takes about 5 minutes.

---

## Step 0: Get an Anthropic API Key

1. Go to https://console.anthropic.com
2. Sign in / create account
3. Go to **API Keys** → **Create Key**
4. Copy it — looks like `sk-ant-api03-...`

---

## Step 1: Deploy the Cloudflare Worker (free)

1. Go to https://workers.cloudflare.com and sign up (no credit card needed)
2. Click **"Create Worker"**
3. Delete the default code and paste the entire contents of **`cloudflare-worker/worker.js`**
4. Click **"Save and Deploy"**
5. Go to **Settings → Variables → Add variable**:
   - Name:  `ANTHROPIC_API_KEY`
   - Value: `sk-ant-...` (your key from Step 0)
   - Check **Encrypt** then click **Save**
6. Your worker URL will look like:
   `https://phonetrace-proxy.YOUR_NAME.workers.dev`

---

## Step 2: Connect the App to Your Worker

Open your app at `https://sirfreeze608.github.io/phonetrace`

The app will show a **"Connect Your Worker"** setup screen on first load.
Paste your worker URL and tap **Save & Continue**.

Your URL is saved in your browser — you only do this once per device.

> To change the URL later: long-press the TRACE button for 1.5 seconds.

---

## Step 3: Push Files to GitHub

Make sure these files are in your `sirfreeze608/phonetrace` repo root:
- `index.html`
- `manifest.json`
- `sw.js`

Go to **Settings → Pages → Source: main → / (root)** → Save.

---

## Step 4: Install on Android

1. Open Chrome on your phone
2. Go to `https://sirfreeze608.github.io/phonetrace`
3. Tap ⋮ → **Add to Home screen**
4. Done — runs like a native app

---

## Security Notes

- Your API key is never in the app's source code
- The worker only accepts requests from `sirfreeze608.github.io`
- Cloudflare free tier: 100,000 requests/day
