# Binance Square API Reference

## 1. Articles List Endpoint

**URL**: `https://www.binance.com/bapi/composite/v3/friendly/pgc/content/article/list`

**Method**: GET

**Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pageIndex | int | Yes | Page number, starting from 1 |
| pageSize | int | Yes | Items per page, max 20 |
| type | int | No | **1** = Trending (approx. 9 pages, ~24h coverage)<br>**2** = Latest (approx. 50 pages, ~42h coverage)<br>**0** = Similar to 2 |

## 2. News Feed Endpoint

**URL**: `https://www.binance.com/bapi/composite/v4/friendly/pgc/feed/news/list`

**Method**: GET

**Parameters**:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| pageIndex | int | Yes | Page number, starting from 1 (supports deep pagination, 80+ pages, ~52h+ coverage) |
| pageSize | int | Yes | Items per page, max 20 |

## Required Headers

| Header | Value |
|--------|-------|
| User-Agent | Standard browser UA string |
| Accept | application/json |
| Accept-Encoding | gzip, deflate, br |
| Referer | https://www.binance.com/zh-CN/square/trending |

## Response Structure (key fields per post)

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique post ID |
| authorName | string | Author display name |
| authorIsVerified | boolean | Whether author is verified |
| cardType | string | Post type (BUZZ_SHORT, ARTICLE, etc.) |
| title | string | Post title (may be null for short posts) |
| content | string | Post body text |
| viewCount | int | Total view count |
| likeCount | int | Total like count |
| commentCount | int | Total comment count |
| shareCount | int | Total share count |
| replyCount | int | Total reply count |
| quoteCount | int | Total quote count |
| date | int | Unix timestamp of post creation (milliseconds or seconds) |
| webLink | string | Web URL to the post |
| hashtagList | array | List of hashtag strings |
| images | array | List of image URLs |
| isFeatured | boolean | Whether post is featured |
| detectedLanguage | string | Detected language code |

## 48-Hour Data Fetching Strategy

To retrieve a complete set of posts from the last 48 hours, the skill combines multiple endpoints:
1. **Latest Posts (`type=2`)**: Primary source for recent chronological posts (covers ~42 hours).
2. **News Feed**: Secondary source to fill gaps and extend coverage beyond 48 hours.
3. **Trending Posts (`type=1`)**: Supplementary source to ensure no high-traffic posts are missed.

All fetched posts are deduplicated using the `id` field, and the highest traffic metrics are kept when duplicates are merged.

## Rate Limiting

No explicit rate limit documented. Recommended: 
- `0.5s` delay between paginated requests
- Stop fetching if 3 consecutive empty pages are encountered
- `5+` minute intervals for continuous monitoring
