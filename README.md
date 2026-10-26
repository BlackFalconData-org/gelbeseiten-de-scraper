# Gelbeseiten Germany Scraper

Extract structured data from [gelbeseiten.de](https://gelbeseiten.de) — gelbeseiten.de, the German Yellow Pages — business name, category, phone, e-mail, website, address, coordinates and rating for any trade, in one city or nationwide. Search by trade and town or paste a search URL. Rows are ready for CRM import and lead lists.

**[Gelbeseiten Germany Scraper on Apify →](https://apify.com/blackfalcondata/gelbeseiten-de-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by ↕️ sort order, 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each company.

**Incremental mode** — Only get new or changed companies since your last run. Content hash per company — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how companies evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N companies (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every company returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured companies from gelbeseiten.de on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor companies, track trends, and analyze market dynamics with structured, deduplicated data from gelbeseiten.de.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per company is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Zahnarzt",
  "location": "Berlin",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Business category, trade or company name — e.g. "Zahnarzt", "IT-Beratung", "Bäckerei". Required if no startUrls provided. Paste a JSON array to search several terms in one run. |
| `location` | string | — | German city, district or postal code — e.g. "Berlin", "München", "10117". Leave empty to search all of Germany. Paste a JSON array for several locations. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste gelbeseiten.de search URLs (https://www.gelbeseiten.de/Suche/<trade>/<city>; without a city = all of Germany), category pages (https://www.gelbeseiten.de/branchen/<trade>/<city>) or business pages (https://www.gelbeseiten.de/gsbiz/<id>, scraped as that one company). A sortierung=bewertung or sortierung=entfernung in the URL sets that URL's order. Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `sort` | enum | `"relevance"` | Order of the search results: best match (relevance), highest rated first (rating) or nearest to the location first (distance). Matters most together with Max Results. A start URL's own sortierung overrides it for that URL. |
| `includeDetails` | boolean | `true` | Fetch each business's full entry and public profile page — adds third-party ratings, the free-text description, weekly opening hours, fax, payment methods, services, languages, social media links, images and recent reviews. Turn off for faster listing-only runs. |
| `descriptionMaxLength` | integer | `0` | Truncate the business description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (search terms, locations, start URLs) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every company returns the same 40-field schema. Missing values are `null` — never omitted.

- `id`
- `name`
- `category`
- `phone`
- `address`
- `postalCode`
- `city`
- `distanceKm`
- `url`
- `portalUrl`
- `sourceUrl`
- `collectedAt`
- `realId`
- `title`
- `location`
- `categories`
- `phones`
- `email`
- `emails`
- `website`
- `street`
- `district`
- `country`
- `latitude`
- `longitude`
- `rating`
- `reviewCount`
- `partnerRatings`
- `openingStatus`
- `description`
- `searchTerm`
- `searchLocation`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per company. Here is a real example from a production run:

```json
{
  "id": "120150000863",
  "name": "svea POLACK ZAHNARZT praxis",
  "category": "Zahnärzte",
  "phone": "08142 5 06 66 62",
  "address": "Kirchenstr. 11, 82194 Gröbenzell",
  "postalCode": "82194",
  "city": "Gröbenzell",
  "distanceKm": 16.4,
  "url": "https://www.gelbeseiten.de/gsbiz/7d9caa6e-0619-4e71-92ea-91acdcbbe033",
  "portalUrl": "https://www.gelbeseiten.de/gsbiz/7d9caa6e-0619-4e71-92ea-91acdcbbe033",
  "sourceUrl": "https://www.gelbeseiten.de/Suche/Zahnarzt/M%C3%BCnchen",
  "collectedAt": "2026-10-24T02:52:07.855Z"
}
```

*Truncated — full records contain 40 fields. See Output fields for the complete schema.*

**[Try Gelbeseiten Germany Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/gelbeseiten-de-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00108 |

See the [actor on Apify](https://apify.com/blackfalcondata/gelbeseiten-de-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape gelbeseiten.de?**
Use this actor on Apify to extract structured data from gelbeseiten.de. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get gelbeseiten.de data as JSON, CSV, or Excel?**
The actor writes each company to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape gelbeseiten.de?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check gelbeseiten.de's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each company gets a content hash. On subsequent runs, only new or changed companies are emitted — saving time, compute, and storage. Expired companies can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Gelbeseiten Germany Scraper](https://apify.com/blackfalcondata/gelbeseiten-de-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
