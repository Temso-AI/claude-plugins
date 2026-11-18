---
name: technical-audit-review
description: Review a website's Temso technical audit and list the fixes that matter for AI crawlers and search engines. Use when the user asks what is wrong with their site, whether AI bots can read it, what changed since the last audit, which pages to fix first, or why AI crawlers get errors on their site.
---

# Technical audit review

Turn a Temso technical audit into a short, ordered fix list. Use only the Temso connector's read tools. Technical audit and traffic tools do not work for pitch projects.

## 1. Pick the project and the audit

1. Call `list-projects` and pick the project; ask if it is not clear.
2. Call `fetch-technical-audits`. Use the newest completed audit unless the user names another one. If there is none, tell the user to start an audit in the Temso app.

## 2. Collect the data

1. `fetch-technical-audit`: site-level checks (`site`), the result of each check across pages (`checkSummary`), and the worst pages (`worstPages`).
   - If `blockedPageCount` is above 0, the site's bot protection blocked the crawler. Say this first, give the `crawlWarning`, and treat all findings as incomplete.
   - Ignored pages and issues are left out and only counted. Mention `ignoredPageCount` and `ignoredIssueCount` when they are above 0.
2. `fetch-technical-audit-diff`: new and fixed issues since the previous audit.
3. `fetch-bot-error-pages` with `categories: ["ai-crawler"]` and `days: 14`: pages that returned 4xx or 5xx errors to AI crawlers. Skip this step if the project has no traffic source connected; the tool then returns no data.
4. For the top 3 checks in your fix list, call `fetch-technical-audit-issue-pages` with the `checkId`. It lists the failing pages, most visited by AI crawlers first.

## 3. Rank the fixes

Rank each failing check with [references/check-priority.md](references/check-priority.md):
1. Its tier: tier 1 first.
2. Within a tier: more failing pages first, then pages that AI crawlers visit more.

Group pages that share a template, such as `/product-signup?plan_id=48` to `?plan_id=60`, and report them as one fix with a page count.

## 4. Write the review

```
# Technical audit: <domain>, <audit date>

<Two sentences: overall state, and the most important fix.>

## Since the last audit
<New and fixed issues from the diff, in one short list. Say if pages were added or removed.>

## Fix first
1. <Check title>: <what is wrong>, <N pages>. Example pages: <up to 3 URLs>. Fix: <one concrete step>.
2. …

## AI crawler errors
<Pages with errors for AI crawlers in the last 14 days, with status codes. Leave out the section if there are none.>

## Lower priority
<Tier 3 checks in one line each.>
```

Rules:
- Use the check titles from the tools, not the check IDs, in the text for the user.
- Give at most 7 items under "Fix first".
- A missing `llms.txt` is not scored by Temso. Do not present it as a problem.
- To ignore a page or an issue, use `ignore-audit-page` or `ignore-audit-error` only when the user asks for it, and confirm the exact rule first.
