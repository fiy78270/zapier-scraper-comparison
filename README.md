# Zapier web scraper: Web Reader, Web Parser, or a dedicated scraping API? What each one does, what it costs, and the limits that decide it

Zapier handles the easy half of web scraping natively. **Web Reader by Zapier** fetches any public page and returns it as Markdown or HTML. **Web Parser by Zapier** takes HTML or JSON you already have and structures it. Neither one touches proxies, CAPTCHAs, or logins.

For pages that fight back, there are two routes. Zapier's own [web scraping automation](https://zapier.com/automations/data-analytics/data-integration/web-scraping) page points to Apify, Browse AI, Firecrawl, and Quick Scraper as the collection layer. Since June 2026, ScraperAPI also runs an official Zapier app that handles proxy and user-agent rotation, CAPTCHA and bot-blocker bypass, and JavaScript rendering as a step inside your Zap.

Which one you need depends on a single question: is the page publicly readable, or does it block you? Everything below is organized around that split, including the credit math that makes a "free" page read expensive at volume.

> **Quick pick.** Public pages at low volume: Web Reader by Zapier, already in your account, one task per read. Structured output from HTML or JSON you already have: Web Parser by Zapier. Blocked, JavaScript-heavy, or geo-restricted targets: a real scraping API step, connected with a single API key. 👉 [Compare ScraperAPI's plans and current pricing](https://www.scraperapi.com/pricing?fp_ref=coupons)

## ScraperAPI's plans, side by side

ScraperAPI prices by API credits, not seats or Zaps. Every tier from Hobby up carries the same feature set (custom headers, CAPTCHA and anti-bot handling, custom sessions, desktop and mobile user agents, automatic retries, unlimited bandwidth, 99.9% uptime guarantee), so the columns that actually separate the plans are monthly credits, concurrent threads, and how precise your geotargeting gets.

| Plan | Credits / month | Concurrent threads | Geotargeting | Price |
| --- | --- | --- | --- | --- |
| Free | 1,000 | 5 | Not specified | $0 |
| Hobby | 100,000 | 20 | US & EU | [Hobby price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | US & EU | [Startup price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | 3,000,000 | 100 | Country-level | [Business price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | 5,000,000 | 200 | Country-level | [Scaling price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | Country-level | [Professional price](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | 21,500,000 (+500K bonus while the offer runs) | 500 | Country-level | $1,975/mo, or $1,777.50/mo billed annually |
| Enterprise | 22,000,000+ | 500+ | Country-level | Custom quote |

Only Advanced carries a fixed public number: **$1,975 per month, or $1,777.50 per month billed annually, which is $21,330 a year and saves $2,370**. Enterprise is quoted per account. The tiers below Advanced are priced against credit volume, and the current figure for each sits on the pricing page, which is why the table links out instead of guessing.

Two rows in that table matter more than the credit counts. Geotargeting moves from US & EU on Hobby and Startup to country-level from Business up, which decides whether your Zap can pull a price from a specific national storefront or only the US and EU versions. Analytics history is capped at the last 30 days on Hobby and Startup and unlimited from Business up.

## Path 1: Web Reader by Zapier, the beta that reads public pages

Web Reader is the closest thing to a native Zapier web scraper. Add it as an action, select **Read Web Page**, and give it a URL. Output Format defaults to Markdown; HTML is the other choice. If the page builds its content with JavaScript, such as a single-page app or a dashboard, set **Wait Time (ms)** as high as 30,000 and the reader holds before capturing. Incomplete output usually means the wait was too short.

The action returns four fields: **Content** in your chosen format, **Title**, **URL** (the final URL after redirects, which is how you find out where a shortened or moved link actually lands), and **Status code**. Pair it with AI by Zapier to summarize or extract fields without adding another app.

The limits decide whether this works for your case:

- It respects `robots.txt`. If a site blocks scraping, the action returns an error, not a partial read.
- Pages behind a login or a paywall are out of reach.
- PDF extraction stops at 200 pages.
- **Each read counts as one task in your Zapier plan.**

That last one adds up. Reading 500 pages a month is 500 tasks, and Zapier task allowances are shared across every step of every Zap you run. Web Reader is also still flagged as beta in Zapier's help documentation, last updated March 20, 2026, so behavior can change under you.

## Path 2: Web Parser by Zapier, for HTML and JSON you already have

Web Parser is a structuring step. You feed it a payload another tool already fetched, and it turns that page into fields.

Zapier's integration page wires it to Quick Scraper, a third-party scraping API whose own description says it handles proxy servers, browsers, and CAPTCHA so you can get HTML from any site with a single API call. The documented configuration is two steps:

1. **Trigger** from Quick Scraper: *Get JSON Response*. Required fields are **URL** and **Access Token**; **Parser Subscription Id** is optional.
2. **Action** in Web Parser: *Parse Webpage*. Required field is **URL to Parse**, plus **Content output format** and a **Continue on failure** toggle.

There's an inconsistency on that page worth knowing before you build. The FAQ mentions triggers called "New Data Scraped" and "Scheduled Scrape Complete," but the supported trigger actually listed in the setup panel is *Get JSON Response*. Configure against the panel, not the FAQ copy.

The same pattern works with any tool that hands Zapier an HTML or JSON payload. The parsing side is portable; the fetching side is what you're choosing between.

## Path 3: The scrapers Zapier itself points to

Zapier's scraping page names Apify, Browse AI, and Firecrawl as the collection layer, with Google Sheets or Coda as the review layer. The documented flow: collect fields from target pages, drop them into a sheet for a person to check, then let Zapier fire alerts, create records, and update tables downstream.

The trigger conditions Zapier lists for that setup are page changes, new records, field gaps, and scrape failures. That set works as a checklist for your own build. If your current process has no trigger for a failed scrape, you learn about the gap when a report looks thin, weeks later.

The manual-scraping problems the same page describes are equally specific: page changes nobody tracked until the dataset drifted, slow response to freshly scraped records, manual exports between the scraper and the docs, and no single view of which sources are covered.

## Path 4: ScraperAPI's official Zapier app

ScraperAPI's app went live in the Zapier App Directory in June 2026, which changes the calculation when your targets block ordinary requests. It exposes three families of actions:

- **Extract Data from URL** sends any URL through ScraperAPI's main endpoint and returns HTML, Markdown, plain text, CSV, or parsed JSON.
- **Get Structured Data** returns parsed JSON for supported sites. At launch: Amazon (product details, search, offers), Google (search, jobs, news, shopping, maps search), eBay (search, product details), Walmart (search, category browse, product details, reviews), and Redfin (for-sale listings, rentals, search, agent profiles).
- **Crawler jobs** as Zap steps: create a crawl from a seed URL, poll its status, cancel it.

Behind those actions, ScraperAPI does the proxy and user-agent rotation, the CAPTCHA and bot-blocker bypass, and JavaScript rendering when a page needs it. Setup is one API key: copy it from the dashboard, add a ScraperAPI step in Zapier, pick an action event, paste the key when prompted. Zapier validates the key once and the connection is reusable across every Zap in the account.

A working Zap looks like this: a trigger from Schedule by Zapier, Google Sheets, or a webhook, then a ScraperAPI action with fields mapped from the previous step, optional parameters such as JS rendering, geotargeting, or output format, then a downstream action that writes to Sheets, Slack, or a database.

👉 [Check which ScraperAPI plan covers your volume before you connect the app](https://www.scraperapi.com/pricing?fp_ref=coupons)

## What a scrape actually costs in credits

Credit pricing only makes sense once you know the per-request rate, and ScraperAPI's rate is not flat. The same plan buys very different amounts of data depending on what you point it at.

| Request type | Credits per request |
| --- | --- |
| Standard page | 1 |
| E-commerce (Amazon, Walmart, eBay) | 5 |
| SERP (Google, Bing, including subdomains) | 25 |
| Social (LinkedIn) | 30 |
| Bot-protection bypass (Cloudflare, DataDome, PerimeterX/HUMAN, Akamai) | +10 |
| JS rendering (`render=true`) | +10 |
| Premium proxies (`premium=true`) | +10 |
| Screenshot (`screenshot=true`) | +10 |
| Premium proxies + JS rendering | 25 |
| Ultra Premium (paid plans only) | 30 |
| Ultra Premium + JS rendering (paid plans only) | 75 |
| Geotargeting, sessions, device type, output format, auto parsing | No extra cost |

Run the arithmetic on your own Zap before picking a tier. A 100,000-credit Hobby plan is 100,000 standard pages, or 20,000 Amazon product pages, or 4,000 Google searches. An hourly SERP check is 25 credits per run, roughly 18,250 credits a month before you add rendering. Bot-protected targets cost 10 extra credits each time ScraperAPI gets past the block.

Hitting a limit works differently from most subscription tools. You get a 429 asking you to bring concurrency back inside your plan's limit; the request is refused rather than queued, and there's no overage charge. On Hobby through Professional you can switch on auto-upgrade so the account rolls into the next tier when credits run out. On Scaling, Professional, Advanced, and Enterprise, pay-as-you-go keeps requests moving at a fixed per-credit rate up to a monthly cap you set. Credits reset at renewal, and annual plans can renew early for a fresh batch.

Every response reports its own cost in the `sa-credit-cost` header. The Domain Cost Estimator in the dashboard prices a URL before you scrape it, and `max_cost` caps any single request. Those three tools answer "will this Zap burn through my plan" better than any estimate.

👉 [See how each ScraperAPI tier's credits map to the sites you're scraping](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Free trial, free plan, and the Zapier free tier are three different things

Worth separating, because the marketing copy blurs them.

ScraperAPI opens with a **7-day trial and 5,000 API credits, no credit card required**. When the trial ends you can stay on the **free plan: 1,000 credits a month and 5 concurrent threads**, or move to Hobby, Startup, Business, Scaling, Professional, Advanced, or a custom Enterprise deal.

Zapier's own "free tier available" line, printed on its app integration pages, refers to Zapier's automation plans. It has nothing to do with ScraperAPI's credits, and neither does Quick Scraper's pricing, which isn't published on the pages reviewed here.

## Matching the setup to the job

Zapier's scraping page lists a set of automation patterns that map cleanly onto the choices above. These are workflow shapes, not product promises from any single vendor:

**Hourly change detection on a public page.** Web Reader plus Schedule by Zapier gets you parsed content every hour; the 30-second wait covers most JavaScript-loaded layouts. If the site blocks `robots.txt` or sits behind bot protection, move the fetch step to ScraperAPI.

**Full text from RSS items that link out.** Web Reader on each new feed item stores the full page so downstream parsing doesn't break on an empty archive. Pages behind a login are still out.

**New PDF links posted on a client site.** Web Reader handles PDFs up to 200 pages. Longer documents need a different tool.

**Tracking newly published listings or prices.** A scrape step followed by a Sheets write is the documented pattern; the trigger condition to watch is field gaps, which catches a selector that silently stopped matching.

**Social scraping jobs that need run IDs and status.** ScraperAPI's LinkedIn rate of 30 credits per request is the number to plan against, and it's the highest standard rate in the table for a reason.

## FAQ

**Is there a free Zapier web scraper?**
Web Reader by Zapier is included in your Zapier plan and costs one task per read. On the ScraperAPI side, the free plan gives 1,000 API credits a month with 5 concurrent threads after a 7-day, 5,000-credit trial.

**Can Zapier scrape a site behind a login?**
Web Reader can't. Login and paywall pages are explicitly out of scope for it, and no amount of wait time changes that.

**Does Web Reader work on JavaScript-heavy pages?**
Yes, with a wait time of up to 30,000 ms. If content comes back incomplete, raise the wait first.

**How many credits does one ScraperAPI call use?**
One for a standard page, 5 for Amazon, Walmart, or eBay, 25 for Google or Bing, 30 for LinkedIn. Bot-protection bypass or JS rendering adds 10 each. Geotargeting and output format are free.

**Is the ScraperAPI Zapier app official?**
Yes. It launched in the Zapier App Directory in June 2026, with documentation on ScraperAPI's own docs site.

**What happens when a ScraperAPI plan runs out of credits?**
The request gets a 429 and is turned away, with no overage charge. You can enable auto-upgrade on Hobby through Professional, or use pay-as-you-go on Scaling, Professional, Advanced, and Enterprise.

## Which setup to build

Match the tool to the obstacle, not to the feature list:

- **Only public pages, a few thousand reads a month:** Web Reader by Zapier. Nothing to buy, nothing to connect.
- **HTML or JSON already arriving from somewhere:** Web Parser by Zapier. It structures; it doesn't fetch.
- **Targets that block bots, need JS rendering, or require country-level geotargeting:** ScraperAPI's Zapier app, or one of the scrapers Zapier names. ScraperAPI earns the slot when you also need structured Amazon, Google, Walmart, eBay, or Redfin output without writing a parser.
- **High-volume SERP or social monitoring:** do the credit math first. Those two categories cost 25 and 30 credits per request, which is 25 to 30 times the price of a standard page.

Two things to confirm before you pay for anything: run your actual target URLs through ScraperAPI's Domain Cost Estimator so you're pricing real pages rather than the 1-credit ideal, and check your concurrency ceiling against how many Zaps you plan to run in parallel. A 429 means you hit the thread limit, not the credit limit.

👉 [Start with ScraperAPI's free plan and compare the paid tiers](https://www.scraperapi.com/pricing?fp_ref=coupons)
