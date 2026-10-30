# billy-dashboard-data

Briefing data for Billy’s AI dashboard. The dashboard framework itself is controlled by Muse (deployed on Vercel); this repo holds ONLY the market-briefing JSON that the dashboard’s briefing section reads.

## Who writes what

- **WorkBuddy** writes `briefing.json` only: morning edition ~08:45 HKT and evening edition ~20:45 HKT. This runs in parallel with Muse’s own morning/evening briefings, which continue to be delivered in chat.
- **Muse** writes everything else (dashboard framework and all other sections). Muse does not write this repo’s files after setup.
- Nobody else should write to this repo.

## How the dashboard reads it

The dashboard fetches:

```
https://raw.githubusercontent.com/billylo824/billy-dashboard-data/main/briefing.json
```

If the file is missing, invalid JSON, or fails schema validation, the dashboard falls back to Muse’s last briefing instead of breaking.

## Schema (strict)

```json
{
  "version": 1,
  "source": "workbuddy",
  "updated_at": "2026-10-30T20:45:00+08:00",
  "edition": "evening",
  "title": "晚報（2026-10-30）",
  "bullets": [
    "最多 5 條，繁體中文（廣東話），每條附核實過嘅數字"
  ],
  "sources": "列出查過嘅來源，例如：Reuters、Investopedia、CME FedWatch"
}
```

- `version` must be `1`.
- `edition` must be `morning` or `evening`.
- `updated_at` must be Hong Kong time in ISO format (`+08:00`).
- `bullets` must be an array of 1–5 strings (6 only for genuinely major breaking news).
- Validate that the file parses as JSON before pushing.

## Content rules

- All figures must be verified against real sources before writing; if unsure, omit the figure.
- No buy/sell recommendations — facts and market conditions only.
- Never write tokens, passwords, or personal data into any file or commit message.

## Token rules

- Access uses a fine-grained personal access token scoped to THIS repo only, with Contents: Read and write.
- The token must never be pasted into any chat, file, or commit.
