# Truelancer Scraper

Extract structured data from [truelancer.com](https://truelancer.com) — a global freelance marketplace with 2M+ freelancers. Three modes in one actor: scrape **job listings** (with budgets, skills, client info), **freelancer profiles** (with ratings, hourly rates, experience level), or **gig services** (with prices, sales, seller info).

**[Truelancer Scraper on Apify →](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi)**

---

## Key features

**3 modes in one actor** — `mode: "jobs"` for project listings, `"freelancers"` for talent profiles, or `"services"` for gig packages. Switch with one input parameter; same incremental, compact, and notification features across all three.

**Detail enrichment** (jobs only) — fetch full job descriptions, client stats (jobs posted, money spent, payment verified), proposal counts, deadline, and total views.

**Incremental mode** — recurring runs emit and charge only for items that are new or whose tracked content changed. Detect expired listings, track repost patterns, and pay-per-event scales linearly with your churn rate.

**Notifications** — push results to **Telegram, Discord, Slack, WhatsApp**, or any custom **webhook** (n8n / Make / Zapier / your own backend) after each run. Pair with incremental + `notifyOnlyChanges` to alert only on new or updated items.

**Compact mode** — AI-agent and MCP-friendly payloads with core fields only. Keeps response size small for LLM workflows and `descriptionMaxLength` lets you cap text length.

**Strict filtering** — combine `jobType`, `categoryId`, `countryCode`, `budgetMin/Max`, `experienceLevel` to narrow results precisely. Every filter is enforced — what you ask for is what you get.

**Normalized output** — ISO millisecond timestamps, structured `countryCode` + `countryName` (via `Intl.DisplayNames`), explicit `priceUnit` enum (`"project"` / `"hour"` / `"package"`), HTML entities decoded, experience level mapped to actor's input enum.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

---

## Use cases

**Lead pipeline for freelancers**
Run `mode: "jobs"` daily with `incrementalMode + notifyOnlyChanges` and a Telegram or webhook destination. Get push-notifications about new fixed-price React jobs over $500 — straight to your phone or CRM, no manual checking.

**Talent sourcing**
Run `mode: "freelancers"` with `experienceLevel: "expert"` and country/skill filters. Build shortlists ranked by rating, projects completed, and hourly rate. Export to spreadsheets or your ATS.

**Gig economy market research**
Run `mode: "services"` to map competitive pricing, niche saturation, and seller performance via `totalSold` and `rating`. Perfect for benchmarking your own services or researching market entry.

**Change monitoring & churn analysis**
Incremental mode detects new, updated, expired, and reposted listings cross-run. Build audit trails of how the marketplace evolves over time.

**AI / LLM training data**
Structured JSON per record is RAG-ready. Use compact mode + `descriptionMaxLength` to control token usage in agent workflows.

---

## Quick start

```json
{
  "mode": "jobs",
  "keyword": "react developer",
  "maxResults": 25,
  "fetchDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | enum | `"jobs"` | `"jobs"` / `"freelancers"` / `"services"` |
| `keyword` | string | — | Search keyword (optional — empty = browse all) |
| `categoryId` | integer | — | Truelancer category ID (e.g. 79 = Virtual Assistant) |
| `countryCode` | string | — | ISO 2-letter country code filter |
| `jobType` | enum | — | Fixed-price or hourly (jobs mode only) |
| `budgetMin` / `budgetMax` | integer | — | Budget range in USD (jobs mode only) |
| `experienceLevel` | enum | — | `entry` / `intermediate` / `expert` (freelancers mode only) |
| `maxResults` | integer | `50` | Result cap (0 = unlimited, max 10000) |
| `fetchDetails` | boolean | `true` | Fetch detail pages for richer fields (jobs mode only) |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars |
| `compact` | boolean | `false` | Core fields only (AI-agent / MCP) |
| `incrementalMode` | boolean | `false` | Emit only new/changed/expired since last run |
| `stateKey` | string | — | Stable identifier for incremental tracking |
| `skipReposts` | boolean | `false` | Drop listings matching prior expired content |
| `telegramToken` + `telegramChatId` | string | — | Push results to a Telegram chat |
| `discordWebhookUrl` | string | — | Discord embed notifications |
| `slackWebhookUrl` | string | — | Slack Block Kit notifications |
| `whatsappAccessToken` + `whatsappPhoneNumberId` + `whatsappTo` | string | — | WhatsApp Cloud API alerts |
| `webhookUrl` + `webhookHeaders` | string / object | — | Generic JSON webhook (n8n / Make / Zapier) |
| `notificationLimit` | integer | `5` | Max items per notification (1-20) |
| `notifyOnlyChanges` | boolean | `false` | Only alert on NEW/UPDATED in incremental mode |

---

## Output fields

Every record returns the same 55-field schema. Mode-specific fields are populated for that mode and `null` for others. Missing values are always `null` — never omitted.

**Identity & meta** — `listingId`, `listingKey`, `mode`, `title`, `url`, `portalUrl`, `searchKeyword`, `scrapedAt`, `source`, `contentQuality`, `detailFetched`

**Content** — `description`, `category`, `subcategory`, `skills[]`, `location`, `countryCode`, `countryName`, `priceValue`, `priceCurrency`, `priceText`, `priceUnit`, `listingType`, `publishedAt`, `deadline`

**Jobs-specific** — `totalProposals`, `totalViews`, `workHours`, `hiringType`, `featured`, `urgent`, `nda`, `clientName`, `clientLocation`, `clientSpent`, `clientJobsPosted`, `clientPaymentVerified`

**Freelancers-specific** — `entityName`, `rating`, `totalReviews`, `projectsCompleted`, `servicesCompleted`, `experienceLevel`, `lastActive`, `isPro`, `isPlus`

**Services-specific** — `totalSold`, `salePercent`, `imageUrl`, `sellerName`, `sellerUrl`

**Incremental** — `changeType`, `isRepost`, `repostOfId`, `repostDetectedAt`

---

## Sample output — jobs mode

```json
{
  "listingId": "e84c5b21...826577f",
  "listingKey": "640538",
  "mode": "jobs",
  "title": "Experienced Ghost Drama Screenwriter Needed for 110 Page Script",
  "description": "Seeking an experienced screenwriter to ghostwrite a 110-page drama screenplay…",
  "category": "Screen & Script Writing",
  "subcategory": "Screenwriting",
  "skills": ["Feature Writing", "Screenwriting"],
  "countryCode": "AU",
  "countryName": "Australia",
  "priceValue": 200,
  "priceCurrency": "USD",
  "priceText": "200 USD",
  "priceUnit": "project",
  "listingType": "Fixed Price",
  "publishedAt": "2026-04-25T09:52:13.000Z",
  "deadline": "2026-05-16",
  "totalProposals": 7,
  "totalViews": 79,
  "hiringType": "immediate",
  "featured": false,
  "urgent": false,
  "nda": false,
  "clientName": "Mary W",
  "clientLocation": "Australia",
  "clientSpent": 0,
  "clientJobsPosted": 2,
  "clientPaymentVerified": false,
  "url": "https://www.truelancer.com/freelance-project/...",
  "contentQuality": "full",
  "detailFetched": true,
  "scrapedAt": "2026-04-25T13:05:36.114Z",
  "source": "truelancer.com"
}
```

## Sample output — freelancers mode

```json
{
  "listingId": "fb7bdc37...d2b0ec",
  "listingKey": "babatundeoladepo",
  "mode": "freelancers",
  "title": "SEO Content Writer | 8 Years Experience",
  "entityName": "Babatunde Oladepo",
  "skills": ["Bloggers", "Content Writers"],
  "location": "Oko Erin, Nigeria",
  "countryCode": "NG",
  "countryName": "Nigeria",
  "priceValue": 3,
  "priceCurrency": "USD",
  "priceText": "3 USD/hr",
  "priceUnit": "hour",
  "listingType": "Freelancer",
  "rating": 5,
  "totalReviews": 585,
  "projectsCompleted": 215,
  "servicesCompleted": 481,
  "experienceLevel": "expert",
  "lastActive": "2026-04-25T09:38:33.000Z",
  "isPro": false,
  "isPlus": false,
  "imageUrl": "https://cdn.truelancer.com/user-picture/280787-...jpg",
  "url": "https://www.truelancer.com/freelancer/babatundeoladepo"
}
```

## Sample output — services mode

```json
{
  "listingId": "482197d5...4034b",
  "listingKey": "187577",
  "mode": "services",
  "title": "White Hat SEO Link Building Package for Ranking on Google Search",
  "description": "We provide white hat seo link building SEO packages…",
  "category": "Internet Marketing",
  "skills": ["YouTube Experts", "Backlink Building Experts"],
  "priceValue": 86,
  "priceCurrency": "USD",
  "priceText": "86 USD",
  "priceUnit": "package",
  "listingType": "Service",
  "rating": 5,
  "totalSold": 296,
  "sellerName": "Jyoti Yadav",
  "sellerUrl": "https://www.truelancer.com/freelancer/dharmendra",
  "url": "https://www.truelancer.com/freelance-service/..."
}
```

**[Try Truelancer Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
|---|---|
| Actor Start | $0.01 |
| Result | $0.002 |

**Example costs:** 10 results = $0.03, 100 results = $0.21, 500 results = $1.01.

See the [actor on Apify](https://apify.com/blackfalcondata/truelancer-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape truelancer.com?**
Use this actor on Apify. Choose your mode (`jobs` / `freelancers` / `services`), set a keyword and result limit, then click Start. No coding required.

**How do I get truelancer.com data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, Keboola, or any custom webhook.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving cost and processing time. Expired and reposted listings are detected automatically.

**Can I get push-notifications when new jobs match my search?**
Yes. Configure Telegram / Discord / Slack / WhatsApp credentials or a generic webhook URL. Pair with `incrementalMode + notifyOnlyChanges` to alert only on new or updated listings.

**Is it legal to scrape truelancer.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check truelancer.com's terms of service for your specific use case.

**Do I need an API key or credentials?**
No — for the scraping itself, just sign up for Apify and start the actor. Notification platforms (Telegram bot token, Discord webhook URL, etc.) require credentials only if you want to use them.

---

## Related products by Black Falcon Data

- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal
- [Naukrigulf Scraper](https://apify.com/blackfalcondata/naukrigulf-scraper?fpr=1h3gvi) — Gulf region job board (UAE/Saudi/Qatar)
- [Wellfound Scraper](https://apify.com/blackfalcondata/wellfound-scraper?fpr=1h3gvi) — Startup jobs and talent
- [Upwork Scraper](https://apify.com/blackfalcondata/upwork-scraper?fpr=1h3gvi) — Adjacent freelance marketplace

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

*Last updated: 2026-04-25*
