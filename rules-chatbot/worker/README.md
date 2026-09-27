# Rules Helper answer service (Cloudflare Worker + Google Gemini)

The web page (`web/index.html`) doesn't answer questions itself. It sends each
question to this small program, `worker.js`, which runs for free on Cloudflare.
The Worker combines the question with the **complete, word-for-word 2026
ruleset** (`kiosk/rules-full.txt` in this repository) and asks Google's Gemini AI
to answer from it. The Gemini key stays hidden inside the Worker and never
appears on the public page.

This is a **one-time setup** done in advance by whoever manages the chatbot.
It's all done in web browsers — no Terminal, no installs. Budget about 20 minutes.

## What you need
- A Google account (for the free Gemini key). No credit card.
- An email address for a free Cloudflare account. No credit card.
- Edit access to this GitHub repository.

## Step 1 — Get a free Gemini API key
1. Go to **aistudio.google.com** and sign in with your Google account.
2. Click **Get API key**, then **Create API key**. Copy the key (a long string of letters and numbers) somewhere safe for Step 3.
3. **Do not** turn on billing for this key's Google Cloud project. Without billing it stays on the free tier: the worst that can happen is it runs out of free requests for the day — it can never charge you.
4. While you're there, note the free-tier limits for the model named in `worker.js` (currently `gemini-2.5-flash`) — especially **requests per day**. That's the realistic ceiling on how many questions the page can answer per day.

## Step 2 — Create the Worker on Cloudflare
1. Go to **dash.cloudflare.com** and create a free account (or sign in).
2. In the left sidebar, open **Workers & Pages** (sometimes under **Compute**), then click **Create** → **Create Worker** (pick the "Hello World" starter if asked).
3. Give it a name, e.g. `socal-swordfight-rules`, and click **Deploy**.
4. Click **Edit code**. Delete everything in the editor, then paste in the entire contents of `worker.js` from this folder. Click **Deploy**.
5. Note the Worker's address shown on its page — it looks like `https://socal-swordfight-rules.YOUR-NAME.workers.dev`.

## Step 3 — Give the Worker your Gemini key
1. On the Worker's page, go to **Settings** → **Variables and Secrets** → **Add**.
2. Type: **Secret**. Name: `GEMINI_API_KEY` (exactly like that). Value: paste your key from Step 1.
3. Save / Deploy.

## Step 4 — Check the Worker is working
Open the Worker's address from Step 2 in any browser. You should see a short
message that includes `"apiKeyConfigured":true` and `"rules":{"ok":true,...}`.
- `apiKeyConfigured` is `false` → redo Step 3 (name must be exactly `GEMINI_API_KEY`).
- `rules` shows `"ok":false` → the ruleset file couldn't be loaded; check `RULES_URL` at the top of `worker.js` points at this repository's `kiosk/rules-full.txt` (only needed if the repository was renamed or moved).

## Step 5 — Connect the web page to the Worker
1. On GitHub, open `web/index.html` and click the pencil (Edit) icon.
2. Find the line `const WORKER_URL = "";` near the bottom and put your Worker's address between the quotes, e.g.
   `const WORKER_URL = "https://socal-swordfight-rules.YOUR-NAME.workers.dev";`
3. Commit. GitHub Pages republishes the site automatically within a minute or two.

## Step 6 — Try it
Open the site (`https://laudy32.github.io/SoCal-Swordfight-Rules-Chatbot/`) and
ask a few questions — see `HOW_TO_TEST.md` for a good list.

## If something goes wrong
The page shows a short message; the Worker's **Logs** tab on Cloudflare shows details.
- *"Requests are only accepted from the SoCal Swordfight rules page"* — the page's address isn't in `ALLOWED_ORIGINS` at the top of `worker.js`. If the site moves to a different address, add it there and redeploy.
- *"free usage limit"* — the Gemini free tier is used up for now (per-minute or per-day). It resets on its own.
- *"the AI model … isn't available"* — Google retired or renamed the model. Change `MODEL` at the top of `worker.js` to a current Flash model listed in Google AI Studio, then redeploy.
- *"key isn't valid" / "rejected this helper's key"* — redo Steps 1 and 3 with a fresh key.

## Good to know
- **Privacy:** participants' questions are sent to Google to be answered, and Google may use free-tier requests to improve its products. The page says this and asks people not to include personal information.
- **Abuse protection is light:** the Worker only accepts requests from the rules page and only answers rules questions with a fixed set of instructions, so it isn't useful as a general free chatbot. Someone determined could still send requests directly; the worst case is using up the free daily quota, never a bill (as long as billing stays off — Step 1).
- **Updating the rules (e.g. 2027):** replace `kiosk/rules-full.txt` in this repository. The Worker picks up the new text automatically within about an hour — nothing to redeploy. (Rebuild the kiosk model separately; see `kiosk/README.md`.)
- **Needs internet:** only the short question and answer travel over the network (the ruleset goes from Cloudflare to Google, not from the phone), so even weak venue WiFi usually works. For fully offline use at the venue, use the kiosk.
