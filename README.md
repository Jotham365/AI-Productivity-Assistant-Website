# Deskclerk

Deskclerk is a lightweight, single-page web app that bundles five small AI-assisted tools for everyday work: drafting emails, summarizing meeting notes, planning tasks, doing quick sourced research, and a general-purpose chat — all wrapped in a warm, ledger-styled interface.

Deskclerk drafts. It doesn't decide. Every tool is meant to produce a first pass you review, edit, and approve — not a finished output you send or act on unchecked.

## Features

- **Email generator** — describe the purpose, recipient, tone, and key points; get a drafted subject line and body.
- **Meeting summarizer** — paste raw notes or a transcript and get back a structured summary: overview, decisions, action items (with owners), and open questions.
- **Task planner** — describe a goal, optional timeframe, and team size; get a phased breakdown of concrete, checkable tasks.
- **Research assistant** — ask a question that needs current or well-sourced information; the assistant searches the web and answers with sources.
- **Quick chat** — a simple conversational fallback for anything that doesn't fit the other tools.

## Tech stack

Deskclerk is intentionally dependency-free:

- Plain **HTML**, **CSS**, and **JavaScript** — no build step, no framework, no bundler.
- **Fraunces** and **Inter** from Google Fonts for the editorial/ledger aesthetic.
- Calls the **Anthropic Messages API** (`claude-sonnet-4-6`) directly for text generation, task planning (via structured JSON output), and web-search-backed research.

## Project structure

```
.
├── index.html      # Markup and page structure
├── styles.css       # All styling (CSS custom properties define the theme)
└── app.js           # App logic: panel navigation, API calls, rendering
```

If you're working from a single combined file instead, all three pieces live inline in one `.html` file — split them out following the structure above if you'd prefer separate files.

## Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/deskclerk.git
   cd deskclerk
   ```
2. Serve the folder with any static file server (needed for the font/API requests to behave correctly), for example:
   ```bash
   npx serve .
   # or
   python3 -m http.server 8000
   ```
3. Open the served URL in your browser.

### ⚠️ API key note

The app's `callClaude()` function in `app.js` sends requests straight to `https://api.anthropic.com/v1/messages` from the browser, with no API key attached in the code. That's fine in an environment where those calls are already authenticated and proxied for you (such as inside a Claude-hosted artifact), but **it will not work as-is from a plain static deployment** — browsers can't call the Anthropic API directly with a bare key, and you should never ship a secret key in client-side JavaScript.

To self-host Deskclerk for real use, put a small backend (or a serverless function) in front of the Anthropic API that:
- holds your API key server-side,
- accepts requests from `app.js`,
- forwards them to `https://api.anthropic.com/v1/messages`,
- and returns the response.

Then update the `fetch` URL in `callClaude()` to point at your own endpoint instead of Anthropic's directly.

## Customization

Most of the visual identity is controlled by CSS custom properties at the top of `styles.css` (`:root`), including colors for ink, paper, amber accents, and text. Adjust those to re-theme the app without touching component styles.

## Responsible use

Deskclerk includes an in-app notice (and an "our approach to responsible use" modal) reminding users to:
- keep confidential or personal information out of inputs,
- verify facts and outputs before relying on them,
- and treat every draft as a starting point, not a final answer.

Keep these guardrails in mind if you extend or redeploy the app.

## License

Add a license of your choice (e.g. MIT) here before publishing.
