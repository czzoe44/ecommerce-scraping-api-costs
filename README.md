# ecommerce scraping api: what 1,000 pages actually costs, which plan fits your volume, and when a general API beats an Amazon-only one

An ecommerce scraping API takes a product URL, search term, or category page and returns structured data — price, title, stock, rating, images — as JSON, so you don't have to maintain your own parsers, proxy pools, and CAPTCHA handling for every store you monitor. If you're comparing options right now, the honest short answer is this: a general-purpose API like ScraperAPI makes sense when you need ecommerce data plus other targets (SERPs, generic sites) under one account; single-retailer APIs are cheaper per page if Amazon is literally all you scrape.

The catch, which most comparison posts skip, is that the sticker price on these tools tells you very little. Credits are consumed at different rates depending on the target site and the features you enable, and the differences are large. Below I'll walk through ScraperAPI's current plans, then show the per-page math that actually determines your bill.

> **Quick picks**
> - Testing the waters: the free tier gives you 1,000 API credits and 5 concurrent connections just for signing up, no card needed. There's also a 7-day trial with 5,000 credits.
> - Price monitoring at moderate volume (up to ~20,000 Amazon pages a month): Hobby at $49/month covers it.
> - Larger catalogs or SERP-heavy workloads: Startup ($149/month, 1M credits) drops the effective cost per page by two-thirds.
> - 👉 [Check current plans and start with 5,000 free trial credits](https://www.scraperapi.com/?fp_ref=coupons)

## How an ecommerce scraping API works in practice

You send one HTTP request with your API key and the target URL, and the service handles the parts that break homegrown scrapers: rotating residential IPs, CAPTCHA solving, JavaScript rendering, retries. Most ecommerce-focused APIs add structured endpoints for the big marketplaces, so instead of parsing raw HTML you get a JSON object with fields like `price`, `title`, and `availability`.

This matters because Amazon, Walmart, and Shopify storefronts run aggressive bot detection — CAPTCHAs, IP bans, JavaScript challenges that stop a plain HTTP client cold. You need residential IPs and real browser rendering to get through reliably, and building that stack yourself is a multi-week project with ongoing maintenance.

ScraperAPI runs a pool of over 40 million IPs across residential, datacenter, and mobile networks in 50+ countries. On top of the raw API, it offers structured data endpoints for Amazon product and search pages, Walmart, Google Shopping, and Google Search — those return parsed JSON directly, so when a marketplace tweaks its page layout, your code keeps working. Independent reviewers who've tested the product at scale single out ecommerce as its strongest area: the Amazon and Walmart structured endpoints, along with Etsy coverage, are where it performs best.

## ScraperAPI plans and pricing (full list)

Here's everything currently listed on the pricing page, including the plans most reviews skip:

| Plan | Monthly price | Annual (per month) | Credits / month | Concurrent threads | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | 5 | [Start free](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | — | 22,000,000+ | 500+ | [Contact sales](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Annual billing takes 10% off every tier. All paid plans include JS rendering, premium residential and mobile IPs, auto-parsing, CAPTCHA and anti-bot handling, unlimited bandwidth, and a 99.9% uptime guarantee. The differences that matter are concurrency, credit volume, and geo-targeting — more on that in the fine-print section.

## The credit math nobody puts in the headline

This is where most reviews stop at "plans start at $49" and move on. Don't budget off the credit count. Budget off the per-page cost for your actual job.

One credit equals one standard page request. But an Amazon page costs 5 credits, Google or Bing costs 25, and LinkedIn costs 30. Then features stack on top:

- `render=true` (JS rendering): +10 credits
- `premium=true` (premium proxy pool): +10 credits
- `ultra_premium=true`: +30 credits, paid plans only
- Sites behind Cloudflare, DataDome, or PerimeterX: +10 credits each time bypass kicks in, and it's auto-detected — you don't choose it

Here's the part buried in the docs: combining features costs more than the sum of the parts. Premium plus JS rendering should logically be +20. It's actually **+25**. Ultra-premium plus JS rendering should be +40. It's **+75** — nearly double the "logical" number. ScraperAPI documents these combined rates, but not prominently, and it's the main reason people report credits vanishing faster than expected.

Not every parameter costs extra, though. `country_code`, `session_number`, `device_type`, `output_format`, `keep_headers`, and `autoparse` are all free — so switching marketplaces by country, keeping sticky sessions, or getting auto-parsed JSON adds nothing to the bill. Cost control lives in the rendering and proxy-tier flags.

There's one more hidden multiplier: DataPipeline, ScraperAPI's no-code pipeline feature, bills at 6 credits for a basic standard request that costs 1 credit through the normal API. E-commerce requests scale better (10 vs 5), and at the heavy end the gap nearly disappears — but if you build no-code pipelines expecting standard API pricing, basic requests burn six times the credits.

Translated into real money per 1,000 pages, using each plan's full monthly allotment:

| Plan | Standard pages (1×) | E-commerce (5×) | JS rendering (10×) | Ultra-premium + JS (75×) |
| --- | --- | --- | --- | --- |
| Hobby ($49) | $0.49 | $2.45 | $4.90 | $36.75 |
| Startup ($149) | $0.15 | $0.75 | $1.49 | $11.18 |
| Business ($299) | $0.10 | $0.50 | $1.00 | $7.48 |
| Scaling ($475) | $0.10 | $0.48 | $0.95 | $7.13 |

Two conclusions fall out of this table. First, plain ecommerce product-page scraping (the 5× column) is reasonably priced: Hobby handles 20,000 Amazon pages a month, Startup handles 200,000. Second, if your targets need JS rendering *and* ultra-premium proxies — heavily protected listings, aggressive anti-bot setups — Hobby's "100,000 credits" shrinks to about 1,333 actual pages. At that rate the entry plan costs more per page than many fully managed scraping services, and you'd want Business or Scaling instead.

Before committing to a tier, use the Domain Cost Estimator in the dashboard: it shows the exact credit cost for any URL you plan to scrape, and you can cap any single request with a `max_cost` parameter. 👉 [Open the pricing page and run your own numbers against the cost estimator](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## The fine print that changes your bill

Four rules sit in the FAQ and docs rather than on the pricing page, and each one affects real usage.

**Credits don't roll over.** Unused credits expire at the end of each billing cycle. If your scraping volume is spiky — heavy some months, quiet others — you're paying for peak capacity every month.

**Running out means stopping, unless you plan for it.** By default, scraping pauses until credits renew. Hobby through Professional plans can turn on auto-upgrade, which bumps you to the next tier mid-cycle when credits run out. Scaling and above get true pay-as-you-go at a fixed per-credit rate, up to a monthly cap you set. If your volume is unpredictable, this distinction alone can decide which tier you pick.

**Concurrency limits return errors, not queues.** Exceed your plan's thread count (20 on Hobby, 50 on Startup, 100 on Business) and the API returns a 429 — the request is turned away rather than queued, with no extra charge. You handle the retry on your side. For a daily job pulling tens of thousands of pages, Hobby's 20 threads can become the binding constraint before the credits do.

**Geo-targeting is restricted on the lower tiers.** Hobby and Startup can only target US and EU locations. Country-level targeting across 50+ countries starts at Business ($299/month). If you need prices from, say, the Japanese or Australian Amazon marketplace, the $49 and $149 plans won't do it.

One more limitation worth knowing: sticky sessions last roughly an hour, by design, for single scrape jobs. If you need the same IP held across days — ad verification, account-management workflows — this is the wrong tool category entirely.

## How it stacks up against dedicated ecommerce APIs

Since the search intent here is usually comparative, here's the landscape, using figures published in a September 2026 hands-on roundup plus the vendors' own current pages. Information only — check each vendor's site yourself before deciding:

| Service | Entry price | Free tier | Notable specifics |
| --- | --- | --- | --- |
| ScraperAPI | $49/mo (100K credits) | 1,000 credits + 7-day/5,000-credit trial | Amazon 5 cr/page; JS render +10; ecommerce + SERP + general under one key |
| Decodo | $19/mo | Free starter plan | 50+ marketplaces, 100+ templates, 99.99% claimed success, 14-day money-back, pay only for successful requests |
| ScrapingBee | $49/mo | 1,000 free credits, no card | General API with ecommerce support |
| Bright Data | Pay-as-you-go, $1.50/1K | 5,000 requests/mo, no monthly commitment | Amazon, Walmart, Target, Best Buy, Etsy |
| Scrapingdog | $40/mo (200K credits) | 200 credits | $0.20 per 1,000 US Amazon pages — lowest entry rate measured |
| Oxylabs | $49/mo | 2,000-request trial | $0.50 per 1,000 parsed Amazon results |

Read that table the right way. If Amazon product pages are 100% of your workload, a dedicated endpoint like Scrapingdog or Oxylabs is genuinely cheaper per page — that's just arithmetic. Where ScraperAPI earns its keep is breadth: the same account and key covers Amazon, Walmart, Etsy, Google Shopping, Google Search, Bing, Zillow, and arbitrary websites. A price-monitoring tool that also tracks SERP positions, or a lead-gen pipeline that mixes marketplace and non-marketplace sources, needs one vendor instead of three. Mixing a $19 budget API for Amazon with a separate SERP provider also means two dashboards, two billing cycles, and two sets of failure modes to debug.

One caution from a documented community complaint: a Reddit user reported being quoted $3,600 for 60 million credits, calculated at 1 credit per Amazon request, then billed under a 5× multiplier that wasn't disclosed upfront — leaving the credits worth only 12 million actual requests. Whether or how representative that is, the lesson is the same one this whole article makes: always price your workload in *actual pages*, not credits.

## Who ScraperAPI is right for (and who should skip it)

It fits a specific profile: a developer or small team scraping standard ecommerce targets at roughly 20,000–200,000 pages a month, who wants proxy rotation, CAPTCHA handling, and JSON parsing handled for them, and whose targets aren't behind the heaviest bot protection. For that use case, the structured Amazon and Walmart endpoints remove an entire category of maintenance work — no parser updates when a page layout changes.

Skip it, or at least reconsider, in three cases. If every request needs ultra-premium proxies plus JS rendering at volume, the effective per-page cost climbs toward $7–$37 per 1,000 depending on tier, where bandwidth-billed residential proxy plans often win. If you need session persistence beyond an hour, look at dedicated residential providers instead. And if you're not comfortable writing code, none of this applies — these are developer tools, full stop.

## FAQ

**Is there a free ecommerce scraping option at ScraperAPI?**
Yes. Signing up gets 1,000 free API credits with 5 concurrent connections, and the pricing page currently advertises a 7-day trial with 5,000 credits and no credit card required. If you need more credits for a test, support can grant additional ones.

**How many Amazon pages can I scrape per month on each plan?**
Divide credits by 5. Hobby: 20,000 pages. Startup: 200,000. Business: 600,000. Scaling: 1,000,000. Those are Amazon-only figures; mixed workloads with SERP or heavily protected targets go through credits faster.

**Do unused credits roll over to next month?**
No. Credits expire at the end of each billing cycle on every plan.

**What happens when I hit my plan's limits?**
Concurrency overage returns a 429 with no charge — retry after dropping back under your thread count. Credit exhaustion pauses scraping until renewal, unless you've enabled auto-upgrade (Hobby through Professional) or pay-as-you-go (Scaling and above).

**Is there a money-back guarantee?**
ScraperAPI doesn't publish a money-back window on the pages reviewed here, so don't count on one. (One competitor in the table above, Decodo, advertises 14-day refunds — a genuine differentiator if refund flexibility matters to you.) What ScraperAPI does offer is a free trial that requires no card, so you can validate the product end to end before spending anything.

## Choosing before you commit

Map your workload first, then pick: pure Amazon price tracking under 20,000 pages a month → Hobby at $49 does it, and the annual rate ($44.10/month) saves $58.80 over a year. Mixed ecommerce + SERP workloads or country targeting outside US/EU → Startup or Business. Unpredictable or growing volume → Scaling, where pay-as-you-go prevents hard cutoffs. Amazon-only at high volume on a tight budget → the dedicated endpoints in the comparison table above will undercut a general API on price, and that's a legitimate reason to go that route.

One last pre-purchase step: grab your target URLs, run them through the dashboard's Domain Cost Estimator, and multiply by your monthly volume. If the resulting number fits comfortably inside a tier's credits, you're set — 👉 [start the 7-day trial with 5,000 credits and test your real targets before paying](https://www.scraperapi.com/?fp_ref=coupons).
