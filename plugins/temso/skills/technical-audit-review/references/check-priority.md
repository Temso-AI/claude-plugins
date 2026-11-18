# Check priority for AI crawlers

Each Temso audit check gets a score: 100 pass, 80 notice, 50 warning, 0 critical, and -1 when the check cannot run yet. Leave out checks with -1.

## Tier 1: AI crawlers cannot read the page

Fix these first. They stop crawlers from getting or trusting the content.

| Check ID | Problem |
|---|---|
| `crawlability.robotsTxt` | robots.txt cannot be fetched or parsed |
| `crawlability.hostReachable` | A host of the site does not respond |
| `aeo.botBrowserDiff` | Bots get a different page than browsers |
| `indexability.robots.meta.noindex` | The page tells search engines not to index it |
| `indexability.canonical.conflict` | The canonical points at a page that is not indexed |
| `meta.canonical.valid` | The canonical URL is invalid or on another domain |
| `internalLinking.broken` | Internal links point at pages that do not exist |
| AI crawler 4xx/5xx errors | From `fetch-bot-error-pages`: crawlers get an error instead of the page |

## Tier 2: AI crawlers cannot understand the page well

| Check ID | Problem |
|---|---|
| `heading.h1.present` | No H1, or more than one |
| `heading.h1.notEmpty` | The H1 has no text |
| `heading.hierarchy.noSkippedLevels` | Heading levels jump, for example H2 to H4 |
| `thinContent.wordCount` | Fewer than 300 words of main content |
| `structuredData.jsonLd` | No valid JSON-LD structured data |
| `meta.title.present` | No page title |
| `meta.title.uniqueAcrossPages` | The same title is on several pages |
| `meta.description.present` | No meta description |
| `i18n.lang.present`, `i18n.lang.valid`, `i18n.lang.contentMatch` | The page language is missing, invalid, or wrong |
| `indexability.sitemap.declared`, `indexability.sitemap.format` | No usable sitemap |

## Tier 3: polish

| Check ID | Problem |
|---|---|
| `meta.title.length`, `meta.description.length` | Title or description too short or too long |
| `meta.canonical.present` | No canonical link |
| `meta.og.title`, `meta.og.description`, `meta.og.image`, `meta.twitter.card` | Social preview tags missing |
| `altText.coverage` | Images without alt text |
| `i18n.hreflang` | No hreflang links for language versions |
| `internalLinking.orphan` | No internal link points at the page |
| `internalLinking.clickDepth` | More than 3 clicks from the homepage |
| `internalLinking.redirected` | Internal links point at redirects |
| `internalLinking.httpLinks` | Internal links use http instead of https |
| `indexability.sitemap.coverage` | Crawled pages missing from the sitemap |

A check that is not in this list goes into tier 2 if its severity is critical, and into tier 3 otherwise.
