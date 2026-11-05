# Wizz Air Flight Price Scraper

Extract structured data from [Wizz Air fares by route and date](https://wizzair.com) — Wizz Air fares by route and date: the lowest price per flight day with currency, fare type and a booking link, for one airport pair or every route from an airport. Structured rows, ready for a spreadsheet or a price alert.

**[Wizz Air Flight Price Scraper on Apify →](https://apify.com/blackfalcondata/wizzair-scraper?fpr=1h3gvi)**

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

**Incremental mode** — Only get new or changed fares since your last run. Content hash per fare — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how fares evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N fares (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every fare returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured fares from Wizz Air fares by route and date on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor fares, track trends, and analyze market dynamics with structured, deduplicated data from Wizz Air fares by route and date.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per fare is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "origin": "BUD",
  "destination": "LTN",
  "tripType": "oneWay",
  "maxResults": 100,
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
| `origin` | string | — | Airport you fly from, as its 3-letter code (e.g. "BUD" for Budapest). Several airports separated by commas ("LTN,LGW") or a city code ("LON" = every London airport Wizz Air serves) also work. Required unless you paste booking URLs below. |
| `destination` | string | — | Airport you fly to, as its 3-letter code (e.g. "LTN" for London Luton). Commas and city codes work as for the departure airport; only routes Wizz Air actually flies are searched. Leave empty to get every route Wizz Air flies from the departure airport(s). |
| `tripType` | enum | `"oneWay"` | One way returns outbound fares only. Round trip also returns the fares for the way back on the same route. |
| `departDateFrom` | string | — | First departure date to scan, as YYYY-MM-DD. Leave empty to start tomorrow. |
| `departDateTo` | string | — | Last departure date to scan, as YYYY-MM-DD. Leave empty to scan 30 days from the first date. Longer ranges are fetched in 30-day pages. |
| `returnDateFrom` | string | — | Round trip only: first date of the way back, as YYYY-MM-DD. Leave both return dates empty to search the way back over the same dates as the departure. |
| `returnDateTo` | string | — | Round trip only: last date of the way back, as YYYY-MM-DD. Leave empty to scan 30 days from the earliest return date. |
| `sortBy` | enum | `"sourceOrder"` | Order of the results, applied before Max Results: cheapest first, earliest travel date first, or the order Wizz Air returns them. Sorting reads every searched day before delivering. |
| `adults` | integer | `1` | Number of adult passengers (1-9). Fares are priced for this party. |
| `children` | integer | `0` | Number of child passengers (0-9). Adults plus children may not exceed 9. |
| `infants` | integer | `0` | Number of infants (0-9), at most one per adult. |
| `maxPrice` | number | — | Only return days whose fare is at or below this amount, in the fare's own currency. Leave empty for no price filter. |
| `wizzDiscountClub` | boolean | `false` | Return the Wizz Discount Club member fare instead of the regular fare. |
| `maxResults` | integer | `100` | Maximum number of fare rows (one per flight day) to return. 0 = no limit. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional - it defaults to a value derived from your route and passenger settings (not the dates, which roll forward), so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip fares whose content matches an expired fare from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit fares whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit one row (changeType EXPIRED) for each fare seen before but no longer offered on a date this run searched. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED fares. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of fares included in each notification message (1-20). |
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
| `descriptionFormat` | enum | `"all"` | Not used by this actor (fares carry no description text); kept for compatibility with shared tooling. |
| `startUrls` | array | — | Wizz Air booking URLs such as https://www.wizzair.com/en-gb/booking/select-flight/SOF/BUD/2026-12-15/null/1/0/0/null. Each one searches that route (and passenger count) for 3 days either side of the date in the URL. When set, the airport and date fields above are ignored. |

---

## Output fields

Every fare returns the same 42-field schema. Missing values are `null` — never omitted.

- `id`
- `direction`
- `travelDate`
- `departureTimes`
- `price`
- `currency`
- `originalPrice`
- `exchangedPrice`
- `exchangedCurrency`
- `priceType`
- `classOfService`
- `hasMacFlight`
- `wizzDiscountClub`
- `adults`
- `children`
- `infants`
- `bookingUrl`
- `portalUrl`
- `departureAirportCode`
- `departureAirportName`
- `departureCountry`
- `departureCountryCode`
- `departureLatitude`
- `departureLongitude`
- `departureCurrency`
- `arrivalAirportCode`
- `arrivalAirportName`
- `arrivalCountry`
- `arrivalCountryCode`
- `arrivalLatitude`
- `arrivalLongitude`
- `arrivalCurrency`
- `searchDateFrom`
- `searchDateTo`
- `source`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `contentHash`
- `changeType`

---

## Sample output

One object per fare. Here is a real example from a production run:

```json
{
  "id": "outbound:BUD:LTN:2026-11-04",
  "direction": "outbound",
  "travelDate": "2026-11-04",
  "departureTimes": [
    "2026-11-04T06:00:00",
    "2026-11-04T08:30:00",
    "2026-11-04T17:30:00"
  ],
  "price": 38490,
  "currency": "HUF",
  "originalPrice": 38490,
  "exchangedPrice": null,
  "exchangedCurrency": null,
  "priceType": "price",
  "classOfService": "JK",
  "hasMacFlight": true
}
```

*Truncated — full records contain 42 fields. See Output fields for the complete schema.*

**[Try Wizz Air Flight Price Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/wizzair-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0009 |
| Result | $0.00135 |

See the [actor on Apify](https://apify.com/blackfalcondata/wizzair-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape Wizz Air fares by route and date?**
Use this actor on Apify to extract structured data from wizzair.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get Wizz Air fares by route and date data as JSON, CSV, or Excel?**
The actor writes each fare to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape Wizz Air fares by route and date?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check Wizz Air fares by route and date's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each fare gets a content hash. On subsequent runs, only new or changed fares are emitted — saving time, compute, and storage. Expired fares can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Wizz Air Flight Price Scraper](https://apify.com/blackfalcondata/wizzair-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
