# Cosmo Care AI — Spaceflight Health Intelligence

Cosmo Care is an educational space-health web app with astronaut wellbeing checkups, manually entered vital readings, equipment guides, health logs, mission-environment information, emergency guidance, and Nova. This upgraded version can connect Nova and the Vitals Review to Anthropic Claude through a secure Vercel serverless endpoint.

## What is included

- Existing Cosmo Care features retained as a single-page app (`index.html`).
- Redesigned dark mission-control interface.
- Claude-powered Nova chat and Claude Vitals Review.
- Server-side Anthropic API integration in `api/chat.js`; the API key is never placed in browser code.
- Offline built-in assistant fallback if Claude is not configured or unavailable.
- Local browser storage for app records; no unsupported `/api/state` network endpoint.
- `.env.example` documents required server environment variables.

## Deploy the complete app (recommended: Vercel)

GitHub Pages only hosts static files and cannot run the `/api/chat` backend. Deploy this repository to Vercel so the frontend and Claude API endpoint share one origin.

1. Push the entire project to a GitHub repository. Do **not** commit `.env` or any real API key.
2. In Vercel, choose **Add New → Project**, import the GitHub repository, and deploy. No build command is required; `index.html` is served as the site and `api/chat.js` is the serverless endpoint.
3. In Vercel Project → **Settings → Environment Variables**, add `ANTHROPIC_API_KEY` with your Anthropic API key. Optionally add `ANTHROPIC_MODEL`; the default is `claude-sonnet-4-5-20250929`.
4. Redeploy after adding the environment variables.
5. Open the Vercel deployment URL, enter the app, open Nova, and send a simple test question. In Vitals, save a reading and choose **Analyse latest readings**.

## Run checks

```bash
npm test
```

The check validates JavaScript syntax in every inline script and the Claude API route, and checks the app's camera permission and absence of the unsupported state endpoint.

## Run locally

Requires Node.js 20+ and the Vercel CLI.

```bash
npm install -g vercel
vercel dev
```

Create a local `.env` file from `.env.example` and add your own key. Keep `.env` out of Git.

## Privacy and safety

- The Anthropic key is read only by the serverless function. Never paste it into `index.html`, commit it, or share it in chat.
- Nova sends the conversation and the app context selected by the app's AI data-sharing setting to Anthropic when live AI is enabled. The Vitals Review sends the last six manually entered readings, without profile name or photo.
- Select **Do not share with Nova** in Settings to prevent AI requests from being sent.
- Do not enter unnecessary personal or sensitive medical information. Review Anthropic's current privacy and API terms before using real health data.
- Readings are manually entered; the app does not connect to medical equipment, validate sensors, monitor astronauts continuously, or diagnose. AI output is educational only and must not replace qualified medical care or approved mission procedures.
- Emergency: contact local emergency services or the mission medical team immediately. Do not wait for the app or AI.

## Existing GitHub Pages deployment

The frontend can still be hosted on GitHub Pages, but live Claude integration will not work there by itself. The recommended full deployment is Vercel, which hosts both `index.html` and `/api/chat` under one domain.
