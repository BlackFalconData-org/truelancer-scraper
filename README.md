# Truelancer Scraper

Extract structured data from [truelancer.com](https://truelancer.com) — truelancer.com — global freelance marketplace with 2M+ freelancers. Extract job listings with budgets and skills, freelancer profiles with ratings and rates, or gig services with prices and reviews.

**[Truelancer Scraper on Apify →](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi)**

---

## Key features

**Multiple input modes** — jobs / projects or freelancer profiles or gig services. Switch modes without re-scraping.

**Detail enrichment** — Fetch full job descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 10.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from truelancer.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from truelancer.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "mode": "jobs",
  "keyword": "web developer",
  "maxResults": 10,
  "fetchDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | enum | `"jobs"` | What to extract: job listings, freelancer profiles, or gig services. |
| `keyword` | string | — | Search keyword (optional — leave empty to browse all listings). |
| `categoryId` | integer | — | Filter by Truelancer category ID (e.g. 79 = Virtual Assistant). |
| `countryCode` | string | — | ISO 2-letter country code filter (e.g. IN, US, GB). |
| `jobType` | enum | — | Filter by fixed-price or hourly projects (jobs mode only). |
| `budgetMin` | integer | — | Minimum budget filter in USD (jobs mode only). |
| `budgetMax` | integer | — | Maximum budget filter in USD (jobs mode only). |
| `experienceLevel` | enum | — | Filter freelancers by experience level (freelancers mode only). |
| `sort` | enum | — | Sort order: latest or popular. |
| `maxResults` | integer | `50` | Maximum number of results to return (0 = unlimited). |
| `fetchDetails` | boolean | `true` | Fetch job detail page for extra fields: description, client stats, proposal breakdown (jobs mode only). |
| `descriptionMaxLength` | integer | `0` | Truncate description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Return core fields only — ideal for AI agent / MCP workflows. |
| `incrementalMode` | boolean | `false` | Compare against previous run and emit only new/changed/expired items. |
| `stateKey` | string | — | Stable identifier for tracked result universe (required when incrementalMode is true). |
| `skipReposts` | boolean | `false` | Skip listings whose content matches an expired listing from a prior run. |

---

## Output fields

Every listing returns the same 51-field schema. Missing values are `null` — never omitted.

- `description`
- `category`
- `skills`
- `location`
- `priceValue`
- `priceCurrency`
- `priceText`
- `listingType`
- `publishedAt`
- `deadline`
- `totalProposals`
- `totalViews`
- `workHours`
- `hiringType`
- `featured`
- `urgent`
- `nda`
- `clientName`
- `clientLocation`
- `clientSpent`
- `clientJobsPosted`
- `clientPaymentVerified`
- `entityName`
- `rating`
- `totalReviews`
- `projectsCompleted`
- `servicesCompleted`
- `experienceLevel`
- `lastActive`
- `isPro`
- `isPlus`
- `totalSold`
- `salePercent`
- `imageUrl`
- `sellerName`
- `sellerUrl`
- `portalUrl`
- `contentQuality`
- `detailFetched`
- `source`
- `changeType`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `listingId`
- `listingKey`
- `mode`
- `title`
- `url`
- `searchKeyword`
- `scrapedAt`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "description": "We are looking for a reliable virtual assistant to handle phone answering, billing tasks, and scheduling appointments.The ideal candidate should be organized, professional, and hav…",
  "category": "Virtual Assistant",
  "skills": [
    "Virtual Assistants",
    "Accountants",
    "Calendar Management",
    "Phone Support"
  ],
  "location": null,
  "priceValue": 500,
  "priceCurrency": "USD",
  "priceText": "500 USD",
  "listingType": "Fixed Price",
  "publishedAt": "2026-04-24T18:15:47.000000Z",
  "deadline": "2026-05-16",
  "totalProposals": 10,
  "totalViews": 85
}
```

*Truncated — full records contain 51 fields. See Output fields for the complete schema.*

**[Try Truelancer Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.02 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape truelancer.com?**
Use this actor on Apify to extract structured data from truelancer.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get truelancer.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape truelancer.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check truelancer.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Truelancer Scraper](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*
