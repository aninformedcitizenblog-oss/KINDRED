# KINDRED site — deploy notes

## Deploy (once)
1. `npm i -g vercel` (or use the Vercel dashboard) -> run `vercel` from this folder -> follow prompts.
2. In Vercel project settings -> Environment Variables:
   - `ANTHROPIC_API_KEY` — required.
   - `KIN_DAILY_REQUESTS` — optional daily circuit-breaker (default 5000).
3. Domains -> add `www.aninformedcitizen.com` (CNAME per Vercel's instructions) plus apex redirect.
4. Deploy to production: `vercel --prod`.

## Architecture
- `index.html` — The Peel. Chat calls `POST /api/kin`; no key or prompt client-side.
- `api/kin.js` — holds the KIN system prompt, model (`claude-haiku-4-5`), payload caps,
  a per-IP limiter (best effort, per-instance) and the daily budget breaker.
- `signal.html` / `dossiers.html` / `reality.html` — static pages; `cleanUrls` maps `/signal` etc.
- `?run=terminal` on `/` boots straight to the terminal (use for bio links / press).

## Weekly ritual (2 minutes, with each drop)
1. Edit `dossiers.html`: lift one redaction bar or revise one line. Do not announce which.
   Scheduled first lifts: the Reynolds "invoice" source; the Larousse location.
2. If the drop changes what KIN may allude to, update the prompt in `api/kin.js` and redeploy.

## Before public launch (the standing checklist)
- Run the 20-session voice-arc seam test against THIS endpoint (Haiku), turns 3-5,
  plus the canon probes (who wrote the novel / Nik's gender / "how does it end").
- Real-device mobile pass; mid-state contrast check.
- Upgrade rate limiting to Upstash Redis if traffic warrants (the in-memory limiter
  resets per serverless instance — fine for a quiet launch, weak under a spike).
- Add analytics (Vercel Analytics is one click) — track: completion to turn 10,
  skip-link usage, outbound clicks to Substack.
- Verify the Centaur/Nature citation on /signal against the original paper.
- /reality is a styled in-voice stub — replace with the real ledger when compiled.
