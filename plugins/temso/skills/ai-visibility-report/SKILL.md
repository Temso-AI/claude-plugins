---
name: ai-visibility-report
description: Write an AI visibility report for a brand tracked in Temso. Use when the user asks how their brand is doing in ChatGPT, Gemini, Perplexity, Copilot, or Google AI answers, wants a weekly or monthly AI visibility update, asks for share of voice against competitors, or needs a client-ready summary of a Temso project.
---

# AI visibility report

Build a report from the Temso connector's read tools. Every number in the report comes from a tool result. Never estimate a number that a tool did not return.

## 1. Pick the project and the period

1. Call `list-projects`. If the user named a brand, match it. If several projects match or none was named, ask which one.
2. If the project has `"available": false`, tell the user that their plan does not include it and stop.
3. If the project type is `pitch`, use the `pitch-snapshot` skill instead. A pitch has one snapshot and no trends.
4. Choose the period:
   - "weekly" or "this week": the last 7 days.
   - "monthly" or "this month": the last 30 days.
   - No period given: the last 14 days, which is the tools' default.
5. Compute the previous period of the same length, ending the day before the current one starts. Pass both with `startDate` and `endDate` (ISO dates), so both windows use the same length.

Keep the same filters (`models`, `countries`, `languages`, `offeringIds`, `personaIds`) on every call when the user asks for a segment, such as "only ChatGPT" or "only Germany".

## 2. Collect the data

Call these for the current period, and the first two again for the previous period:

| Data | Tool and arguments |
|---|---|
| Share of voice, own brand vs competitors | `fetch-share-of-voice` (the own brand has `isOwn: true`) |
| Visibility, source presence, mentions per brand | `fetch-brand-rankings` with `limit: 10` |
| Visibility per AI platform | `fetch-performance-breakdown` with `dimension: "models"` |
| Sentiment of the own brand, per platform | `fetch-sentiment` (default dimension is models) |
| Most cited domains | `fetch-top-domains` with `limit: 10` |
| Open recommended actions | `fetch-actions` with `status: "open"`, `sortBy: "priority"`, `sortOrder: "desc"`, `limit: 5` |

Only when the negative share is above 5% or the user asks about negative mentions, also call `fetch-sentiment-sources` with `sentiment: "negative"` and `limit: 5`.

Field meanings:
- **Visibility**: the share of AI responses that mention the brand.
- **Share of voice (SoV)**: the brand's share of all brand mentions.
- **Source presence**: the share of responses that cite the brand's own site.

## 3. Write the report

Use this structure. Keep it short enough to read in two minutes.

```
# AI visibility report: <brand>, <start date> to <end date>

<Two sentences: the main change and the main reason, from the data.>

## Key numbers
| Metric | This period | Previous | Change |
| Visibility | 6% | 5% | +1 pp |
| Share of voice | 5.9% | 5.2% | +0.7 pp |
| Mentions | 924 | 810 | +14% |
| Source presence | … | … | … |
| Positive / negative sentiment | 50% / 1.4% | … | … |

## Competitors
<Top 5 brands by share of voice, plus the own brand with its rank. Name the competitor that gained the most.>

## By AI platform
<One row per platform: visibility and positive sentiment. Name the strongest and weakest platform.>

## Sources AI assistants cite
<Top 5 domains. Mark the own domain and domains that mention competitors but not the brand.>

## Next actions
<Up to 5 open Temso actions, highest priority first, each with its target URL.>

<One line: period, filters, and the number of responses the numbers come from.>
```

Rules:
- Give changes in percentage points (`pp`) for shares and in percent for counts.
- If a period has fewer than 50 responses, say that the numbers are not yet reliable.
- If the previous period has no data, leave out the Previous and Change columns, and say why.
- Do not paste raw JSON or tool output.
- This skill only reads data. If the user wants to change prompts, competitors, or actions, confirm the change with them first.
- End with one link to the project in the Temso app: `https://app.temso.ai`.
