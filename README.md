# Daily AI Digest

An n8n workflow that pulls your AI newsletters from Gmail every morning, summarizes them with Gemini, and sends the digest to your WhatsApp.

## How it works

1. **Schedule Trigger** — Kicks off daily at 10 AM.
2. **Gmail** — Fetches the last 24 hours of emails from a curated list of AI newsletters (NLP News, Last Week in AI, The Neuron, Superhuman, Superintel, ByteByteGo, AI Valley).
3. **Gemini AI** — Sends the email snippets to Gemini 3.1 Flash Lite, which returns a short summary (under 1500 characters) with Gmail links for each newsletter.
4. **WhatsApp via Twilio** — Delivers the digest straight to your phone. If there are no newsletters that day, you'll get a quick "No Newsletters today" message instead.

## Workflow diagram

```
Schedule Trigger → Gmail → Build Prompt (JS) → Gemini API → Extract Digest (JS) → Has newsletters?
                                                                                      ├─ Yes → WhatsApp (digest)
                                                                                      └─ No  → WhatsApp ("No Newsletters today")
```

## Setup

You'll need the following before importing this into n8n:

- **Gmail OAuth2** — Connect your Google account so n8n can read your inbox.
- **Gemini API Key** — Get one from [Google AI Studio](https://aistudio.google.com/apikey). Replace `YOUR_GEMINI_API_KEY` in the HTTP Request node's URL.
- **Twilio Account** — Set up a Twilio account with WhatsApp sandbox or a verified number. Update the `from` and `to` phone numbers in both Twilio nodes.

### Import steps

1. Open your n8n instance.
2. Go to **Workflows → Import from File**.
3. Select `Daily AI Digest.json`.
4. Update the credentials and phone numbers mentioned above.
5. Activate the workflow.

## Customization

- **Add or remove newsletters** — Edit the Gmail node's filter query to include/exclude sender addresses.
- **Change the schedule** — Adjust the `triggerAtHour` value in the Schedule Trigger node.
- **Tweak the summary** — Modify the prompt in the first Code node to change summary length, format, or style.
