# VARAI

A lightweight AI-assisted financial management interface for small businesses, combining a conversational (Hinglish) chat workflow with a cash-flow snapshot, credit scoring, and basic invoice/payment tracking.

## Overview

VARAI is a single-page, WhatsApp-style chat interface aimed at Indian small and medium businesses. Instead of digging through spreadsheets or a traditional dashboard, the owner talks to VARAI in Hindi, English, or Hinglish and gets answers about GST dues, pending payments, cash flow, and loan eligibility.

The app ships with a **Mock/Demo Mode** that runs entirely offline using sample business data, so it can be explored without any API key or backend. It also supports wiring up a real LLM provider (Claude, GPT, or Gemini) for live conversational responses, using a key the user supplies themselves in the browser.

All state — chat history, invoices, corrections, credit score — lives in the browser's `localStorage`. There is no backend and no database; this is a frontend prototype.

## Features

- Conversational financial assistant (Hindi / English / Hinglish)
- Business cash-flow snapshot (bank balance, pending receivables, GST due, profit)
- GST/payment tracking with an on-time filing history
- Credit-health scoring based on GST compliance, overdue payments, bank balance, and profitability
- Loan eligibility simulation (bank vs. NBFC ranges)
- Invoice/payment workflows with confirm/cancel actions
- Bill upload simulation (mock OCR extraction)
- CSV export for handing data to an accountant
- Chat history and export
- Responsive layout (desktop + mobile sidebar)
- Mock/Demo Mode that works with no API key

## Tech Stack

- HTML5
- CSS3
- JavaScript (vanilla, no framework or build step)
- Browser `localStorage` for persistence
- REST APIs (Anthropic, OpenAI, Google Gemini — optional, user-supplied keys)

## Architecture

```
User Interface (chat + sidebar)
        │
        ▼
Application State (in-memory, persisted to localStorage)
        │
        ▼
Business Logic (credit scoring, GST/cash-flow calculations, response parsing)
        │
        ▼
AI Provider (Claude / GPT / Gemini) ── or ── Mock Provider (offline, deterministic)
        │
        ▼
Local Storage
```

Everything runs client-side in a single HTML file. The "AI provider" layer is swappable: Mock Mode uses a rule-based response generator seeded from the current business state; the live providers send the same state (as a system prompt) to the selected model's chat completion endpoint.

## AI Provider Integration

The sidebar lets you choose between Claude (Anthropic), GPT (OpenAI), Gemini (Google), or Mock/Demo Mode. For the three live providers, you paste your own API key into the browser — it's stored only in that browser's `localStorage` and is sent directly from the browser to the provider's API. It is never written to this repository and never touches any server of ours (there is no server).

**No API key is included in this repository**, and none is required to use the app — Mock/Demo Mode is the default and reproduces the full conversational flow with canned-but-dynamic responses generated from the sample business data.

If you want to try a live provider, get your own key from the provider (e.g. the Anthropic Console) and paste it into the sidebar. Because the key lives in browser storage and is used directly from client-side JavaScript, treat this as a personal/demo setup rather than something you'd expose to other users — a production version of this feature would proxy those calls through a backend so keys never reach the browser.

## Demo

This is a static HTML app with no build step.

1. Clone the repository.
2. Open `index.html` directly in a browser, or serve it with any static file server (e.g. `python3 -m http.server`).
3. It starts in Mock/Demo Mode — no setup required.
4. Optionally, select a different provider in the sidebar and paste in your own API key to try live responses.

## Project Structure

```
VARAI/
├── index.html      # entire application (markup, styles, and logic)
├── README.md
└── .gitignore
```

The project intentionally stays single-file — it's a frontend prototype, not a production app, and splitting it up didn't seem worth the added complexity yet.

## Security

- No API keys or credentials are included in this repository.
- Any API key you enter is stored only in your own browser's `localStorage` and sent directly to the provider you chose — it is never persisted anywhere else.
- All business data (company name, GSTIN, parties, amounts) is fictional and generated for demo purposes.
- If you fork this and add a real backend, keep provider credentials server-side rather than in client-side JavaScript.

## Limitations

- Credit scoring and loan eligibility are simplified demonstration calculations, not real underwriting decisions.
- Bill scanning is simulated (randomly generated vendor/amount) — there is no real OCR.
- Payment reminders and loan applications are simulated; no message or lending provider is actually integrated.
- All financial figures are sample data, reset by "Clear History."
- Live AI responses depend on the API key and provider you supply and are subject to that provider's own accuracy limitations.

## Future Improvements

- Move live API calls behind a small backend proxy so keys never touch the browser.
- Replace mock bill scanning with real OCR (e.g. Tesseract or a vision-capable model).
- Persist state to a real database instead of `localStorage`.
- Add multi-business / multi-user support.
- Real WhatsApp Business API integration for actual reminders.

## License

No license has been added yet — all rights reserved by default until one is chosen.
