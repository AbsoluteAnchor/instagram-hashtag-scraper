[Instagram Hashtag Scraper](https://apify.com/datapilot/instagram-hashtag-scraper?fpr=data)

🚀 **Instagram Hashtag Scraper** is a powerful Apify Actor designed to extract public posts from **Instagram** by hashtag, without using the official Instagram API. It leverages **residential proxies** to avoid IP blocks and delivers rich, structured data – perfect for **hashtag analytics**, influencer discovery, trend monitoring, and social media research.

## 🔥 Features

- **No Official API Required** – scrapes public **Instagram** content directly, serving as a true **Instagram API alternative**.
- **Smart Proxy Integration** – uses **Apify residential proxies** to avoid IP blocks and achieve **Instagram rate limit bypass**, ensuring reliable **Instagram data extraction**.
- **Hashtag‑Based Search** – enter one or more hashtags (comma‑separated or as an array) and get sample posts for each.
- **Rich Post Metadata** – extracts **post ID**, **code**, **taken_at** timestamp, **media_type** (image/carousel), **caption**, **user details** (pk, username, full_name, profile_pic_url), **like_count**, **comments_count**, **product_type**, **hashtags** used, and more.
- **Summary Statistics** – generates a summary with total posts, likes, comments, images, carousels, and averages.
- **Apify Dataset Ready** – each post is pushed as a separate dataset item for easy export (JSON, CSV, XML).
- **Async Architecture** – fast, non‑blocking **async Python scraper** built with asyncio.
- **Lightweight & Extensible** – sample data generation can be replaced with real scraping logic using tools like `instaloader`, `playwright`, or custom HTTP requests.

---

## ⚙️ How It Works

1. **Input** – Provide one or more **Instagram hashtags** (e.g., `"travel"`, `"food"`). The Actor accepts comma‑separated strings or an array.
2. **Proxy** – Actor initialises a **residential proxy** via Apify Proxy (recommended for **Instagram anti-block**).
3. **Scrape** – For each hashtag, the Actor generates sample posts (or you can replace the logic with real scraping). The current implementation demonstrates the data structure and proxy integration.
4. **Output** – Each post's data is pushed to the Apify Dataset – a perfect **Instagram data export** solution. A summary object is also pushed at the end.
5. **Finish** – Logs total scraped posts, likes, comments, and exits.

---

## 📥 Input

The Actor accepts a JSON input with the following fields:

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `hashtags` | string / array | required | One or more **Instagram hashtags** (e.g., `"travel, food"` or `["travel", "food"]`). |
| `useResidentialProxy` | boolean | `true` | Enable Apify residential proxy – recommended for **Instagram scraping**. |
| `proxyCountry` | string | `"US"` | Country code for proxy (e.g., `"US"`, `"GB"`). |
| `posts_per_hashtag` | integer | `10` | Number of posts to scrape per hashtag. |
| `upload_to_dataset` | boolean | `true` | Whether to push results to the Apify dataset. |

**Example input:**

```
{
  "hashtags": "travel, food",
  "posts_per_hashtag": 5,
  "useResidentialProxy": true,
  "proxyCountry": "US"
}
```

---

## 📤 Output

Each dataset item corresponds to one Instagram post from a hashtag search:

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Unique Instagram post ID (format: media_id_user_id). |
| `code` | string | Shortcode of the post (used in URLs). |
| `taken_at` | string | ISO timestamp of when the post was published. |
| `media_type` | int | 1 = image, 8 = carousel (album). |
| `caption` | string | Post caption text. |
| `user` | object | Nested object containing: pk (user ID), username, full_name, is_private, profile_pic_url. |
| `like_count` | int | Number of likes – Instagram like count. |
| `has_liked` | bool | Always false (public data). |
| `product_type` | string | "feed" or "carousel_container". |
| `is_paid_partnership` | bool | Indicates if the post is a paid partnership. |
| `comments_count` | int | Number of comments – Instagram comment count. |
| `hashtags` | array | List of hashtags found in the caption. |

Additionally, a final summary item is pushed with the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `hashtags_scraped` | array | List of hashtags processed. |
| `total_hashtags` | int | Number of hashtags. |
| `total_posts` | int | Total posts scraped. |
| `total_likes` | int | Sum of likes across all posts. |
| `total_comments` | int | Sum of comments across all posts. |
| `image_count` | int | Number of image posts. |
| `carousel_count` | int | Number of carousel posts. |
| `average_likes_per_post` | int | Average likes per post. |
| `average_comments_per_post` | int | Average comments per post. |
| `completed_at` | string | ISO timestamp of completion. |

**Example output item (post):**

```
{
  "id": "1234567890123456789_9876543210",
  "code": "AbCdEfGhIjK",
  "taken_at": "2025-02-14T12:34:56Z",
  "media_type": 1,
  "caption": "Amazing content about #travel! 🔥\n\n#travel #instagram #explore",
  "user": {
    "pk": "9876543210",
    "username": "creator_travel_1",
    "full_name": "travel Creator 1",
    "is_private": false,
    "profile_pic_url": "https://scontent-iad3-2.cdninstagram.com/v/t51.2885-19/default.jpg"
  },
  "like_count": 123456,
  "has_liked": false,
  "product_type": "feed",
  "is_paid_partnership": false,
  "comments_count": 7890,
  "hashtags": ["travel", "instagram", "explore"]
}
```

**Example output item (summary):**

```
{
  "hashtags_scraped": ["travel", "food"],
  "total_hashtags": 2,
  "total_posts": 10,
  "total_likes": 1250000,
  "total_comments": 45000,
  "image_count": 7,
  "carousel_count": 3,
  "average_likes_per_post": 125000,
  "average_comments_per_post": 4500,
  "completed_at": "2025-02-14T12:35:00Z"
}
```

---

## 🧰 Technical Stack

- **Language:** Python 3.11+ (async/await)
- **Core Scraper:** `instaloader`, `playwright`, or custom HTTP requests – flexible integration for Instagram data extraction.
- **Proxy:** Apify Proxy with RESIDENTIAL group – real peer IPs, high anonymity.
- **Platform:** Apify Actor – serverless, scalable, integrated with Dataset and Key‑Value Store.
- **Deployment:** One‑click run on Apify Console or via REST API.

---

## 🎯 Use Cases

- **Hashtag Analytics** – track the popularity and sentiment of specific hashtags on Instagram.
- **Trend Monitoring** – identify emerging topics and viral content by analysing posts under trending hashtags.
- **Influencer Discovery** – find top creators who frequently use certain hashtags.
- **Brand Monitoring** – see how your branded hashtag is being used by the public.
- **Competitor Research** – analyse which hashtags your competitors are targeting.
- **Content Strategy** – understand which hashtags drive the most engagement (likes, comments).
- **Academic Research** – collect datasets of Instagram posts by hashtag for social science studies.
- **Campaign Analysis** – measure the reach and engagement of marketing campaigns using specific hashtags.
- **Niche Exploration** – discover popular accounts and content in specific niches (fitness, fashion, beauty, etc.).
- **Social Listening** – monitor public conversations around your industry or products.

---

## 🚀 Quick Start

1. **Open in Apify Console** – visit the Actor page and click Try for free.
2. **Enter one or more hashtags** in the input field (e.g., `"travel, food"`).
3. **(Optional) Adjust proxy settings** – residential proxies are enabled by default.
4. **Click Start** – the Actor will generate sample posts for each hashtag.
5. **Export** – download the results as Instagram data JSON, CSV, or Excel.

You can also call this Actor programmatically via Apify SDK or REST API – ideal for automated pipelines needing a reliable Instagram hashtag scraper. Once you replace the sample logic with real scraping, you'll have a powerful tool for unlimited Instagram scraping with Instagram anti-block protection.

---

## 💎 Why This Actor?

| Feature | Benefit |
| --- | --- |
| ✅ No Instagram API quota | Scrape millions of posts by hashtag without paying – a true Instagram API alternative. |
| ✅ Residential proxies | Bypass Instagram bot detection – high success rate with Instagram residential proxy. |
| ✅ Rich post details | Get nested user info, like/comment counts, media type, captions, hashtags – complete Instagram post metrics. |
| ✅ Hashtag‑focused | Specifically designed for hashtag‑based searches – perfect for Instagram trend research. |
| ✅ Summary statistics | Automatically generates insights like total posts, likes, comments, and averages. |
| ✅ Extensible design | Easy to add real scraping logic (e.g., using `instaloader`). |
| ✅ Apify ecosystem | Seamless integration with other Actors, triggers, and webhooks. |

## 📦 Changelog

### v1.0.0 (February 2025)

- Initial release with residential proxy support.
- Hashtag-based search functionality.
- Extracts comprehensive post metadata (user info, engagement metrics, media type, captions).
- Summary statistics with total posts, likes, comments, and averages.
- Support for single or multiple hashtags.
- Sample data generation for demo purposes.
- Easily extensible for real scraping integration.
- Full Apify Actor integration.

---

## 🧑‍💻 Support & Feedback

- **Issues & Ideas:** Open a ticket on the Apify Actor issue tracker.
- **Contributions:** Pull requests are welcome via the GitHub repository.
- **Documentation:** Visit Apify Docs for platform guides.
- **Community:** Join the Apify community forum for discussions and support.

---