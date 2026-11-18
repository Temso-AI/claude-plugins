---
name: visibility-improvement-plan
description: Make a prioritized plan to improve a brand's visibility in AI answers, from Temso data. Use when the user asks why competitors appear more often in ChatGPT, Gemini, or other AI assistants, what content to create, which sites to get listed on, or what to do next to raise share of voice for a Temso project.
---

# Visibility improvement plan

Find where the brand loses in AI answers and why, then turn that into a short list of concrete work. Use only the Temso connector's read tools. Every finding comes from a tool result.

## 1. Pick the project

Call `list-projects` and pick the project the user means; ask if it is not clear. This skill needs a project of type `project`. Content gap, action, and traffic tools do not work for pitches. For a pitch, use the `pitch-snapshot` skill.

Use the last 30 days (`days: 30`) unless the user gives a period.

## 2. Find where competitors win

1. `fetch-share-of-voice`: the own brand (`isOwn: true`) and the top 3 competitors by share of voice.
2. `fetch-prompt-brand-rankings` with those 3 competitors by name in `competitorBrandIds` and `competitorLeadsBy: "visibility"`. These are the buyer questions where a competitor is mentioned more often than the brand.
3. `fetch-performance-breakdown` with `dimension: "models"`, to see on which AI platforms the brand is weakest.

## 3. Find why

Collect the evidence for the questions from step 2:

| Question | Tool |
|---|---|
| Does the brand's site answer the question at all? | `fetch-content-gaps` for the worst prompts, then `fetch-content-gap-detail` with `promptId` for up to 5 of them. A `depthScore` of 0 means no page on the site answers the question. |
| Which sources do the AI assistants trust? | `fetch-top-domains` with `limit: 15`. Domains with `brandMentions` that include competitors but not the brand are outreach targets. |
| Which pages mention competitors? | `fetch-cited-pages` with the competitors in `brandIds` and `limit: 20`. |
| What do the AI assistants search for before they answer? | `fetch-fan-out-queries` with `limit: 15`. These are topics for new content. |
| Does Temso already recommend work? | `fetch-actions` with `status: "open"`, `sortBy: "priority"`, `sortOrder: "desc"`, `limit: 10`. |

If a content gap result has `crawlWarning` or `note`, tell the user and treat the content findings as incomplete.

## 4. Write the plan

Group the work into three types, and rank each item by expected impact: how many tracked questions it affects and how often the source is cited.

```
# Plan to raise <brand>'s AI visibility

<Two sentences: where the brand loses most, and the biggest reason.>

## Where competitors win
<Up to 5 buyer questions, each with the competitor that wins and the brand's visibility.>

## 1. Content to create or improve
<Each item: the question or fan-out topic, the page to improve (URL) or "new page", and what the page must cover.>

## 2. Sources to appear in
<Each item: the domain or page, why it matters (how often AI assistants cite it, which competitors it mentions), and the step: listing, review, guest article, or correction.>

## 3. Open Temso actions
<The highest-priority open actions that support the items above, with their target URL.>

## How to measure
<The metric and questions to check again in 2-4 weeks, for example visibility on the questions above.>
```

Rules:
- Keep the plan to at most 10 items in total.
- Name the evidence for each item, such as "cited in 18% of answers" or "depthScore 0".
- Do not create, change, or dismiss actions, prompts, or content pieces unless the user asks. If they ask, confirm the exact change first.
