# Summarizer — The WebPage with AI

> AI-powered webpage summarizer — extract the gist of any article, save summary history (with page title + timestamp), and read comfortably with dark mode.
> *Polished, lightweight Chrome extension (Manifest V3) + a web demo option.*

---

![Demo GIF placeholder](./assets/demo.gif)
**Live demo:** `https://YOUR-DEMO-URL` ← *(insert when available)*
**Chrome Web Store:** `https://YOUR-STORE-URL` ← *(insert when available)*

---

## Quick elevator pitch

Summarizer is a compact, practical Chrome extension that extracts article text from the current tab, sends it to an AI backend to produce concise summaries, and stores a short history of recent summaries (page title + date/time + text). It includes a dark/light mode toggle, rounded modern UI, and export/copy conveniences — built to be resume-ready and deployable as a web demo.

---

## Key features

* Summarize any webpage into three modes (customizable): Pulse / Streamline / Essence (or your chosen labels).
* Save the last N summaries to a local history (stores `title`, `date/time`, `summary`).
* Dark / Light mode with persistent preference and an icon that toggles (🌙 / ☀️).
* Copy summary to clipboard; download as `.txt` (optional).
* Minimal, responsive UI with modern rounded buttons and subtle animations.
* Manifest V3 compatible (background service worker, content script message passing).
* Easy to demo as a web app (for recruiters) or install as an unpacked Chrome extension.

---

## Tech stack

* Frontend: HTML, CSS (vanilla), JavaScript (ES6+)
* Chrome APIs: `chrome.storage`, `chrome.tabs`, content scripts, service worker (background)
* AI backend: Google Generative Language (Gemini) or any compatible LLM REST endpoint
* Optional demo hosting: Vercel / Netlify / GitHub Pages (requires small adjustments)

---

## What’s in the repo

```
/manifest.json
/background.js           # service worker (onInstalled, initial setup)
/content.js              # extracts article text from current page
/popup.html              # UI when extension icon is clicked
/popup.js                # popup logic: summarize, history, copy, dark-mode
/options.html            # API key and settings page
/options.js
/icon.png
/README.md
/assets/                 # screenshots, demo gif (not included)
```

---

## Install & run (developer / recruiter friendly)

### 1) Quick local install (unpacked)

1. Clone the repo:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
```

2. Open Chrome → `chrome://extensions/`
3. Enable **Developer mode** (top-right)
4. Click **Load unpacked** → select the cloned repo folder
5. Click the extension icon to open the popup.

> Notes: for first use, open **Options** and paste your Gemini/API key.

---

### 2) Web demo version (for recruiters — no extension install)

If you host a web demo on Vercel or GitHub Pages, the UI behaves the same as the popup but:

* There is no access to the active tab DOM (so the demo reads text from a textarea or paste box).
* To protect your API key, use a serverless function (Vercel Function / Netlify Function / simple backend) that holds the key and proxies requests to the AI API.

---

## How it works — architecture overview

1. **Popup** requests summarization → sends a message to the active tab content script: `{ type: "GET_ARTICLE_TEXT" }`.
2. **Content script** extracts readable article text (tries `<article>`, falls back to `p` tags — consider Readability.js for better extraction).
3. Extracted text returns to popup. Popup calls `getGeminiSummary(text, mode, apiKey)` to generate summary.
4. Popup saves a history entry to `chrome.storage.local`:

```json
{
  "title": "<tab.title>",
  "date": "2025-08-12 10:23:00",
  "type": "Pulse",
  "summary": "…"
}
```

5. Dark mode state saved in `chrome.storage.local` and applied on popup load.

---

## Permissions (manifest.json)

The extension requests the minimum required:

```json
"permissions": ["scripting","activeTab","storage"],
"host_permissions": ["<all_urls>"]
```

Explain these to recruiters: `activeTab` and `scripting` are used to inject/communicate with the content script; `storage` persists API key and history.

---

## Security & privacy

* **No user data leaves your control** except when you explicitly send page text to the configured AI API.
* **API key is stored locally** in `chrome.storage.sync` — do not commit it to source control.
* Recommend: use a paid/managed backend for production to avoid exposing keys in client code. For the demo, use serverless proxy functions to keep keys safe.

---

## UX / design notes

* Buttons are **rounded (pill-shaped)** with subtle shadow and hover lift for a modern feel.
* Dropdown & options use `font-weight: 500` for legibility.
* Dark mode toggles icon and keeps contrast accessible.

---

## Resume-friendly bullets (copy-paste)

* Built a Chrome Extension (Manifest V3) that extracts article text and generates AI summaries using remote LLMs.
* Implemented persistent local history (title + timestamp + summary), dark/light mode, and accessible UI with modern styling.
* Used Chrome Storage API, content scripts, message passing, and a Manifest V3 service worker.

---

## Screenshots / GIF

![summary1](https://github.com/user-attachments/assets/3a2f80a6-602d-4ab0-8b6e-01c8d3ecdd06)
![2 deatiled and brefif](https://github.com/user-attachments/assets/93d616f0-77a5-44b8-8b6e-f9956af37daf)


---

## Troubleshooting & FAQ

* **Q:** The extension returns “Could not extract article text.”
  **A:** Try on a standard article page. For complex pages, enable “Readability” in future versions or manually paste text in demo.
* **Q:** My API key isn’t working.
  **A:** Verify your Gemini/API key is valid and not rate-limited. For web demo, ensure your serverless proxy is set up correctly.
* **Q:** How many summaries are saved?
  **A:** Default keeps last 10 (configurable in `popup.js`).

---

## Contributing

PRs welcome. Suggested starter tasks:

* Integrate Readability.js for content extraction.
* Add “clear history” and “export history” functions.
* Improve UI accessibility and keyboard navigation.

---

## License

`MIT` — use, modify, and share. Please keep attribution in `README.md`.

---

## Contact

Developer: **YOUR NAME**
GitHub: `https://github.com/YOUR_USERNAME`
Email: `you@example.com`

