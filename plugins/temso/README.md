# Temso

**Market your brand to AI agents.**

Millions of customers now discover new products and services through AI agents. Temso helps you market your brand to those agents.

Measure how often AI recommends you compared to your competitors, and exactly how it describes you. Then improve. Temso audits your website and tracks the AI agents that visit, so you can fix the pages where they get stuck. It builds a content plan from what your buyers ask, using your Google Search Console and Bing data. It also finds the directories, publications and conversations AI trusts, so your PR goes where it counts.

Ask in plain language and get your live Temso data and next steps in Claude. This plugin requires a [Temso](https://www.temso.ai) account.

## What you can do

- Track AI visibility, share of voice and sentiment
- See how AI describes your brand
- Find the sources and pages AI cites
- Monitor AI crawler traffic and error pages
- Audit your website
- Analyze Google Search Console and Bing data
- Find content gaps
- Publish content pieces
- Analyze backlinks and backlink gaps
- Get recommended actions
- Build pitch snapshots for prospects
- Manage competitors, offerings, personas and prompts

## Try asking

- "How often does AI recommend us compared to our competitors?"
- "Which pages do AI agents get stuck on, and how do I fix them?"
- "What content should we write next, based on our Search Console data?"

## What you need

- A Temso account with at least one project. [Start here](https://www.temso.ai) if you do not have one.
- A paid Claude plan to add plugins on claude.ai and in Cowork, or Claude Code.

## Set it up

1. Add the plugin. On claude.ai and in Cowork, find **Temso** under **Customize → Plugins**. In Claude Code, run:

   ```
   /plugin install temso --marketplace Temso-AI/claude-plugins
   ```

2. Open the plugin's **Connectors** tab and connect **Temso**. Claude opens the Temso sign-in page.
3. Sign in and choose the projects Claude may use. Your role in the Temso workspace decides whether Claude can only read data or can also change it.

You can revoke the connection at any time in Temso under **Organization settings → MCP → My connections**.

## Skills

| Skill | Ask Claude, for example |
|---|---|
| **AI visibility report** | "Give me this week's AI visibility report for Acme." |
| **Pitch snapshot** | "Set up a pitch for acme.com in the US and UK." |
| **Visibility improvement plan** | "Why does Globex show up more than us in ChatGPT, and what should we do?" |
| **Technical audit review** | "What should we fix on our site so AI crawlers can read it?" |

You can also ask Claude any other question about your Temso data. See the [tool reference](https://docs.temso.ai/docs/mcp/tools) for every tool the connector has.

## Data

- The plugin itself has only instructions and stores no data.
- When you ask a question, Claude calls the Temso connector at `https://app.temso.ai/api/v1/mcp`. The connector returns data from the Temso projects you allowed when you connected.
- With write access, Claude can change project configuration, such as prompts, competitors, offerings, personas, and pitches. The skills ask you before any change.
- Tool results go to Anthropic so that Claude can answer you. The plugin sends nothing to any other service.

Privacy policy: <https://www.temso.ai/legal/privacy-policy>

## Support

- Documentation: <https://docs.temso.ai/docs/mcp>
- Contact: <https://www.temso.ai/contact>
