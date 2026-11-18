---
name: pitch-snapshot
description: Build or read a Temso pitch, a one-time AI visibility snapshot of a prospective client's brand. Use when an agency user wants to win a prospect, asks to set up a pitch or snapshot for a website, wants to continue a pitch setup, or asks for the results of a pitch to present to a prospect.
---

# Pitch snapshot

A pitch shows a prospect how AI assistants talk about their brand today. The prospect is the brand of the pitch, not the user's own brand. Creating a pitch needs the organization admin or manager role and the `mcp:write` permission on the Temso connection.

## Find out what the user wants

- **New pitch:** go to "Set up a pitch".
- **Continue a setup:** call `list-projects`, pick the pitch, then call `fetch-pitch-setup`. It returns the saved steps and the next step. Continue from there.
- **Results of a pitch:** go to "Read the snapshot".

## Set up a pitch

The steps must run in this order. Each list the user approves replaces the saved list.

1. **Prospect.** Ask for the prospect's website if the user did not give it. Call `create-pitch` with `website`. Pass `name`, `description`, or `aliases` only if the user gave them; Temso writes the description from the website. The result has suggested markets.
2. **Markets.** Show the suggested markets: countries or cities, plus languages. Ask the user to confirm or change them. Save the approved markets with `update-pitch-setup` and `markets`. Use countries or cities, not both.
3. **Offerings.** Call `suggest-pitch-offerings`. Show the list as name plus one line each. Save the approved list with `update-pitch-setup` and `offerings`.
4. **Personas.** Call `suggest-pitch-personas`. Show the list. Save the approved list with `update-pitch-setup` and `personas`. An empty list skips personas.
5. **Prompts.** Call `suggest-pitch-prompts`; it can take a minute. Show the prompts as a numbered list with their intent type. Save the approved list (at most 10) with `update-pitch-setup` and `prompts`. The result adds monthly search volume per prompt.
6. **Finish.** Tell the user that the setup cannot change after this step, and that later prompt changes need a rerun in the Temso app, which the plan limits. When they confirm, call `finish-pitch-setup`.

Rules for the setup:
- Show each suggestion and save only what the user approves. Do not save a list the user has not seen.
- Saving markets, offerings, or personas deletes the saved prompts, because prompts are built from them. Warn the user before you change an earlier step after the prompts are saved.
- If a tool says that the brand already has a live pitch or that the daily pitch limit is reached, tell the user and stop.

## Read the snapshot

1. Call `fetch-pitch-snapshot`. Report the status:
   - `collecting`: answers are still arriving. Give the answers received per platform and suggest that the user checks again in a few minutes.
   - `ready` or `partial`: continue. For `partial`, say that some answers are missing.
   - `failed`: tell the user that no answers came back and suggest a rerun in the Temso app.
2. Collect the results. Pitch analytics return only the latest snapshot, so do not pass dates and do not report trends:
   - `fetch-share-of-voice` and `fetch-brand-rankings` with `limit: 10`. Competitors come from the snapshot answers.
   - `fetch-performance-breakdown` with `dimension: "models"`.
   - `fetch-sentiment` and `fetch-brand-perception` with `mode: "byBrand"`.
   - `fetch-top-domains` with `limit: 10`.
   - `fetch-prompt-brand-rankings` with the top 3 competitors by name in `competitorBrandIds` and `competitorLeadsBy: "visibility"`. It returns the prompts where a competitor beats the prospect.

Traffic, backlink, technical audit, content gap, content piece, action, and tracked URL tools do not work for pitches. Do not call them.

## Write the pitch summary

Write for the prospect, so a person can paste it into a deck or email:

```
# How AI assistants see <prospect>

<Two sentences: how often AI assistants mention the prospect, and who they recommend instead.>

## Share of voice
<Prospect vs the top 5 competitors.>

## By AI platform
<Visibility per platform. Name where the prospect is missing.>

## Questions where competitors win
<3-5 buyer questions from the snapshot where the prospect is not mentioned, with the brands that are.>

## What AI assistants say about <prospect>
<Sentiment and the top perception attributes.>

## Sources AI assistants trust
<Top 5 cited domains.>

## Where to start
<3 concrete opportunities from the data, such as the sources to appear in and the questions to answer on the site.>
```

Do not invent numbers or claims about the prospect. Every number comes from a tool result.
