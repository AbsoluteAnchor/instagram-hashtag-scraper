[Instagram Hashtag Scraper](https://apify.com/khadinakbar/instagram-hashtag-scraper?fpr=data)

# Scrape Instagram Hashtag Posts with Engagement Metrics

Extract posts, likes, comments, captions, and author profiles from any Instagram hashtag. Returns clean JSON with **pre-calculated engagement rates** — ready for influencer discovery, brand monitoring, and AI pipelines.

No coding required. Enter a hashtag, get structured data in minutes.

**Compatible with:** Apify MCP Server (Claude, ChatGPT), LangChain, Make.com, Zapier, n8n, and REST API.

---

## What data can you extract from Instagram hashtags?

| Field | Type | Example |
| --- | --- | --- |
| `hashtag` | string | `"fitness"` |
| `post_url` | string | `"https://instagram.com/p/CxABC123/"` |
| `caption` | string | `"Morning grind 💪 #fitness #gym"` |
| `caption_hashtags` | array | `["fitness", "gym", "motivation"]` |
| `mentions` | array | `["gymshark", "niketraining"]` |
| `tagged_users` | array | `["coach_mike", "fitbuddy99"]` |
| `like_count` | integer | `4821` |
| `comment_count` | integer | `203` |
| `reshare_count` | integer | `45` |
| `video_view_count` | integer | `89300` |
| `play_count` | integer | `142000` (Reels) |
| `video_duration_secs` | number | `28.5` |
| `is_reel` | boolean | `true` |
| `media_type` | string | `"IMAGE"` / `"VIDEO"` / `"CAROUSEL_ALBUM"` |
| `posted_at` | datetime | `"2026-03-25T08:14:00.000Z"` |
| `location_name` | string | `"Central Park, New York"` |
| `location_id` | string | `"213385402"` |
| `music_title` | string | `"As It Was"` (Reels) |
| `music_artist` | string | `"Harry Styles"` (Reels) |
| `author_username` | string | `"johnfitness"` |
| `author_full_name` | string | `"John Smith"` |
| `author_follower_count` | integer | `24800` |
| `author_is_verified` | boolean | `false` |
| `author_is_business` | boolean | `true` |
| `author_profile_url` | string | `"https://instagram.com/johnfitness/"` |
| `engagement_rate` | number | `20.3` (%) |

The **engagement rate** is pre-calculated as `(likes + comments) / followers × 100` — no post-processing needed. A rate above 3% is good; above 6% is excellent.

The actor extracts **both posts and Reels**, including Reel-specific metadata like play count, video duration, and music track information.

---

## How to scrape Instagram hashtag posts

1. Click **"Try for free"** to open the actor in Apify Console
2. Enter one or more **hashtags** (without the # symbol): `["fitness", "yoga"]` — or paste full Instagram hashtag URLs via **Start URLs**
3. Add your **Instagram session cookies** in the `sessionCookies` field (required — see instructions below)
4. Set **max posts per hashtag** (default: 50)
5. Choose **post type**: Top posts (highest engagement) or Recent posts (newest)
6. *(Optional)* Set a **date filter** (`dateFrom`) to only return posts from a specific date onwards
7. *(Optional)* Set a **minimum likes** threshold to filter out low-engagement posts
8. Enable **Fetch author details** if you want engagement rates and follower counts
9. Click **"Start"** and download your results as JSON, CSV, or Excel

### How to get your Instagram session cookies

Instagram requires authentication to show hashtag data. Your session cookies let the actor browse Instagram as you, without sharing your password.

1. Open **Chrome** and log into [instagram.com](https://www.instagram.com)
2. Press **F12** to open DevTools → click the **Application** tab
3. In the left sidebar expand **Cookies** → click `https://www.instagram.com`
4. Find the cookie named `sessionid` — right-click → Copy Value
5. For best results, copy the full cookie string: click the **Console** tab and run:

```
document.cookie
```
6. Paste the entire output into the `sessionCookies` field

The two most important cookies are `sessionid` and `csrftoken`. Your cookies are only used to authenticate requests — they are never stored or shared.

### How to get Instagram hashtag data with engagement rates

Enable the **"Fetch author details"** option to unlock engagement rate calculation. The actor makes an additional API call per unique author to retrieve their follower count, then automatically computes the engagement rate for every post.

This is the key differentiator from generic Instagram scrapers — you get a **single clean dataset** ready for influencer shortlisting, sorted by engagement rate.

### Using with AI agents (Claude, ChatGPT)

Connect via the [Apify MCP Server](https://apify.com/apify/actors-mcp-server) and ask naturally:

> "Find me the top 50 fitness influencers with engagement rates above 5%"
> 
> 
> 
> 
> "Get 100 recent posts from the #streetphotography hashtag and sort by likes"
> 
> 
> 
> 
> "Scrape hashtags #yoga and #meditation and find creators with under 50k followers"

The AI will automatically select and run this actor, returning structured results ready for further analysis.

---

## Use cases for Instagram hashtag scraping

**Influencer Discovery:** Find creators in any niche ranked by engagement rate. Filter by follower count to target nano, micro, or macro influencers. Export directly to your outreach CRM.

**Brand Monitoring:** Track what content is being published under your brand hashtag or campaign tag. Identify top-performing posts and understand what resonates with your audience.

**Competitor Analysis:** Monitor competitors' branded hashtags to see who's talking about them and what's performing best. Spot gaps in their content strategy.

**Content Research:** Discover trending topics and co-hashtags within a niche. The `caption_hashtags` field shows which hashtags top creators use together.

**AI Pipelines:** Feed structured post data into ChatGPT, Claude, or RAG systems for automated influencer scoring, content gap analysis, or campaign reporting.

**Lead Generation:** Identify businesses and creators using specific industry hashtags. Cross-reference `author_is_business` to target commercial accounts for B2B outreach.

---

## How much does it cost?

| Use case | Posts | Estimated cost |
| --- | --- | --- |
| Quick research | 50 posts | ~$0.15 |
| Influencer shortlist | 200 posts | ~$0.60 |
| Campaign monitoring | 500 posts | ~$1.50 |
| Large dataset | 2,000 posts | ~$6.00 |

Pricing is **$0.003 per post** (pay-per-result) — you only pay for actual posts extracted. No monthly fees, no minimums.

---

## API & Integration

### REST API — how to scrape Instagram hashtags programmatically

```
curl -X POST "https://api.apify.com/v2/acts/khadinakbar~instagram-hashtag-scraper/runs" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "hashtags": ["fitness", "gym"],
    "maxPostsPerHashtag": 100,
    "postType": "RECENT",
    "fetchAuthorDetails": true
  }'
```

### JavaScript

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_TOKEN' });

const run = await client.actor('khadinakbar/instagram-hashtag-scraper').call({
    hashtags: ['fitness', 'gym'],
    maxPostsPerHashtag: 100,
    postType: 'RECENT',
    fetchAuthorDetails: true,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items[0].engagement_rate); // → 20.3
```

### Python

```
from apify_client import ApifyClient

client = ApifyClient('YOUR_TOKEN')

run = client.actor('khadinakbar/instagram-hashtag-scraper').call(
    run_input={
        'hashtags': ['fitness', 'gym'],
        'maxPostsPerHashtag': 100,
        'postType': 'RECENT',
        'fetchAuthorDetails': True,
    }
)

items = list(client.dataset(run['defaultDatasetId']).iterate_items())
# Sort by engagement rate
top_creators = sorted(items, key=lambda x: x.get('engagement_rate') or 0, reverse=True)
```

**Integrations:** Apify MCP Server, LangChain, Make.com, Zapier, n8n, Google Sheets, Airtable, HubSpot

Export scraped data, run the scraper via API, schedule and monitor runs, or integrate with other tools using Apify's built-in connectors.

---

## FAQ

**Q: Do I need an Instagram account or login?**
A: Yes — Instagram has required authentication to view hashtag data since 2024. You don't need to enter your username or password in the actor; instead, you copy your session cookies from your browser (takes about 60 seconds) and paste them into the `sessionCookies` field. See the **"How to get your Instagram session cookies"** section below for step-by-step instructions.

**Q: What is a good engagement rate on Instagram?**
A: Industry benchmarks: Under 1% = low, 1–3% = average, 3–6% = good, 6%+ = excellent. Nano-influencers (under 10k followers) typically see 5–10% rates. Mega-influencers (1M+) often see under 1%.

**Q: What's the difference between Top and Recent posts?**
A: **Top posts** are selected by Instagram's algorithm based on engagement and are shown prominently on the hashtag page. **Recent posts** are all posts in reverse chronological order. Use Top for finding proven high-performers; use Recent for real-time monitoring.

**Q: Can I scrape multiple hashtags at once?**
A: Yes — pass an array: `["fitness", "gym", "workout", "crossfit"]`. The actor processes each hashtag sequentially and clearly tags each post with its source hashtag.

**Q: How fast does it scrape?**
A: Approximately 40–80 posts per minute, depending on proxy speed and whether author details are fetched. The actor paces requests to avoid rate limiting.

**Q: Can I schedule recurring runs?**
A: Yes — use Apify's built-in scheduler to monitor hashtags daily, weekly, or at any custom interval. Combine with webhooks to push new posts to Slack, Airtable, or your CRM automatically.

**Q: Is web scraping Instagram legal?**
A: This actor only collects publicly available data visible to anyone without an account. See [Apify's guide on web scraping legality](https://blog.apify.com/is-web-scraping-legal/).