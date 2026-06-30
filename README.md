[Instagram Hashtag Scraper](https://apify.com/sovereigntaylor/instagram-hashtag-scraper?fpr=data)

# Instagram Hashtag Scraper

Scrape Instagram posts by hashtag without an API key. Extract captions, likes, comments, authors, images, videos, locations, hashtags, and mentions from any public hashtag.

## Use Cases

- **Influencer Marketing** — Find top-performing posts and influencers for any hashtag. Identify potential brand ambassadors by analyzing engagement rates, follower counts, and content quality across your target hashtags.
- **Trend Analysis** — Monitor hashtag growth and content trends over time. Track which hashtags are gaining momentum, what type of content performs best, and how engagement patterns shift across seasons and events.
- **Competitor Research** — Analyze your competitors' branded hashtags and campaigns. See what content resonates with their audience, benchmark engagement metrics, and discover gaps in their content strategy.
- **Content Strategy** — Discover what types of posts perform best for specific hashtags. Use data on top-performing captions, image styles, posting times, and engagement patterns to optimize your own content calendar.
- **Market Research** — Understand audience sentiment and behavior around topics, products, or brands. Aggregate post data to identify customer pain points, popular product features, and emerging market trends.
- **UGC Discovery** — Find user-generated content featuring your brand or products. Identify authentic customer posts that can be repurposed (with permission) for marketing campaigns and social proof.

## Features

- No API key or Instagram login required
- Multiple extraction strategies (embedded JSON, meta tags, DOM parsing)
- Google search fallback when Instagram blocks direct access
- 12 rotating user agents to avoid detection
- Automatic pagination for large scrapes
- Deduplication by post shortcode
- Pay-per-event pricing ($0.003 per post)
- Proxy support (residential proxies recommended)
- Sorts by recent or top posts
- Optional comment extraction

## Input

| Field | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `hashtags` | array | Yes | — | Hashtags to scrape (without #). Example: `["travel", "photography"]` |
| `maxPosts` | integer | No | 500 | Maximum posts to scrape per run (1–10,000) |
| `sortBy` | string | No | `recent` | Sort order: `recent` or `top` |
| `includeComments` | boolean | No | `false` | Scrape comments for each post (slower) |
| `proxyConfiguration` | object | No | — | Proxy settings. Residential proxies recommended |

### Example Input

```
{
  "hashtags": ["travel", "wanderlust"],
  "maxPosts": 100,
  "sortBy": "top",
  "includeComments": false,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Output

Each post is saved as a dataset item with the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `shortcode` | string | Unique Instagram post identifier |
| `caption` | string | Post caption text |
| `likes` | number | Number of likes |
| `comments` | number | Number of comments |
| `author` | string | Username of the post author |
| `authorFollowers` | number | Author's follower count (when available) |
| `imageUrl` | string | URL of the post image |
| `videoUrl` | string | URL of the video (if video post) |
| `isVideo` | boolean | Whether the post is a video/reel |
| `date` | string | ISO 8601 timestamp of when the post was created |
| `location` | string | Location tag (when available) |
| `hashtags` | array | Hashtags found in the caption |
| `mentions` | array | @mentions found in the caption |
| `postUrl` | string | Direct URL to the Instagram post |
| `sourceHashtag` | string | Which input hashtag this post was found under |
| `scrapedAt` | string | ISO 8601 timestamp of when the post was scraped |
| `extractionStrategy` | string | Which extraction method was used |

### Example Output

```
{
  "shortcode": "C3xK9mNPqRs",
  "caption": "Exploring the hidden gems of Bali #travel #wanderlust #bali",
  "likes": 15234,
  "comments": 342,
  "author": "travelguru",
  "authorFollowers": 125000,
  "imageUrl": "https://instagram.com/...",
  "videoUrl": null,
  "isVideo": false,
  "date": "2026-02-28T14:30:00.000Z",
  "location": "Ubud, Bali",
  "hashtags": ["#travel", "#wanderlust", "#bali"],
  "mentions": [],
  "postUrl": "https://www.instagram.com/p/C3xK9mNPqRs/",
  "sourceHashtag": "travel",
  "scrapedAt": "2026-03-02T10:00:00.000Z",
  "extractionStrategy": "sharedData"
}
```

## How It Works

The scraper uses multiple strategies to extract data, falling back automatically if one method fails:

1. **Embedded JSON** — Parses `window._sharedData`, `__additionalDataLoaded`, and Relay runtime data embedded in Instagram pages. This provides the richest data including exact like/comment counts, author details, and timestamps.
2. **Instagram API** — Attempts to use Instagram's internal API endpoints (`/explore/tags/{hashtag}/?__a=1`) for structured JSON responses with full post metadata.
3. **Meta Tags** — Extracts OpenGraph and Twitter Card meta tags from post pages. Provides image URLs, descriptions, and basic engagement data.
4. **DOM Parsing** — Falls back to parsing the HTML structure with Cheerio when JSON data is unavailable. Extracts post links, images, and captions from the page layout.
5. **Google Search Fallback** — When Instagram blocks direct access, searches Google for `site:instagram.com/p/ #hashtag` to discover post URLs, then fetches each individual post page for enrichment.

## Proxy Configuration

Instagram actively blocks scrapers. For best results, use residential proxies:

```
{
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

Without proxies, the scraper will still attempt to extract data but may encounter rate limiting or login walls. The Google search fallback helps mitigate this.

## Pricing

This actor uses pay-per-event pricing:

- **$0.003 per post scraped** — You only pay for successfully extracted posts
- No charge for failed requests or deduplicated posts
- Comments are included at no extra charge when `includeComments` is enabled

## Limitations

- Instagram frequently updates its page structure. Extraction strategies are designed to be resilient but may need updates over time.
- Rate limiting may reduce the number of posts scraped in a single run. Use proxies for best results.
- Some post metadata (like exact follower counts) may not always be available depending on the extraction strategy used.
- Very new or very niche hashtags may have limited posts available.
- Comment extraction requires additional requests and significantly increases run time.

## Support

For issues, feature requests, or questions, contact: [ricardo.yudi@gmail.com](mailto:ricardo.yudi@gmail.com)

## Integration — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("sovereigntaylor/instagram-hashtag-scraper").call(run_input={
    "searchTerm": "instagram hashtag",
    "maxResults": 50
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{item.get('title', item.get('name', 'N/A'))}")
```

## Integration — JavaScript

```
import { ApifyClient } from 'apify-client';
const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const run = await client.actor('sovereigntaylor/instagram-hashtag-scraper').call({
    searchTerm: 'instagram hashtag',
    maxResults: 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach(item => console.log(item.title || item.name || 'N/A'));
```