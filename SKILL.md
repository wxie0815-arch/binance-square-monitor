---
name: binance-square-monitor
description: "Monitor Binance Square trending posts and track real-time traffic data including view count, like count, comment count, and share count. Use for scraping Binance Square hot posts, monitoring crypto social media engagement, fetching 48-hour full historical data, tracking post traffic changes over time, and generating traffic reports."
---

# Binance Square Trending Posts Monitor

Scrape and monitor posts from Binance Square, tracking view count, like count, comment count, and share count in real time. Supports single fetching, continuous monitoring, and 48-hour full historical data extraction.

## Installation

```bash
gh repo clone wxie0815-arch/binance-square-monitor
cd binance-square-monitor
```

## Quick Start

The main script is at `scripts/binance_square_monitor.py` relative to the skill root directory.

### 48-Hour Full Data Fetch

Scrape all posts from the past 48 hours using multiple data sources with automatic deduplication.

```bash
python3 scripts/binance_square_monitor.py fetch-48h --output ./binance_square_data
```

### Single Fetch (One-time Snapshot)

```bash
python3 scripts/binance_square_monitor.py fetch --pages 3 --output ./binance_square_data
```

### Continuous Monitoring

```bash
python3 scripts/binance_square_monitor.py monitor --interval 300 --pages 3 --output ./binance_square_data
```

## Workflow

Monitoring Binance Square trending posts involves these steps:

1. Fetch posts via API (`fetch`, `fetch-48h`, or `monitor` command)
2. Review terminal output for traffic summary table
3. Check CSV/JSON files for historical data and change tracking
4. Analyze trends using the changes log or the 48-hour comprehensive report

## Script Usage

The main script is `scripts/binance_square_monitor.py`. It requires only the `requests` package (pre-installed).

### Commands

**`fetch-48h`** — Scrape all posts from the past 48 hours (or custom time range):

| Flag | Default | Description |
|------|---------|-------------|
| `--hours` | 48 | Number of hours of historical data to fetch |
| `--page-size` | 20 | Posts per page |
| `--output` / `-o` | None | Output directory (omit for terminal-only) |
| `--format` | all | Output format: csv, json, all, print |

**`fetch`** — Single snapshot of trending posts:

| Flag | Default | Description |
|------|---------|-------------|
| `--pages` | 3 | Number of pages to fetch (20 posts/page) |
| `--page-size` | 20 | Posts per page |
| `--output` / `-o` | None | Output directory (omit for terminal-only) |
| `--format` | all | Output format: csv, json, all, print |

**`monitor`** — Continuous monitoring with change tracking:

| Flag | Default | Description |
|------|---------|-------------|
| `--interval` | 300 | Seconds between snapshots |
| `--pages` | 3 | Pages per snapshot |
| `--page-size` | 20 | Posts per page |
| `--output` / `-o` | auto | Output directory |
| `--max-snapshots` | 0 | Max snapshots (0 = unlimited) |

### Output Files

| File | Content |
|------|---------|
| `48h_full_data.csv` | Full dataset from the `fetch-48h` command |
| `48h_full_data.json` | Structured JSON with stats from the `fetch-48h` command |
| `48h_report.md` | Comprehensive summary report for 48h data |
| `trending_posts.csv` | All snapshots with full traffic data (monitor/fetch) |
| `trending_posts.json` | Structured JSON with snapshot grouping (monitor/fetch) |
| `changes_log.csv` | Delta values between consecutive snapshots (monitor) |
| `monitor_report.md` | Summary report (generated on exit for monitor) |

## Data Fields Collected

Each post record contains:

| Field | Description |
|-------|-------------|
| post_id | Unique post identifier |
| author | Author display name |
| view_count | Total views |
| like_count | Total likes |
| comment_count | Total comments |
| share_count | Total shares |
| reply_count | Total replies |
| quote_count | Total quotes |
| post_time | Post creation time (UTC) |
| hashtags | Associated hashtags |
| web_link | Direct URL to post |

## Programmatic Usage

Import functions directly for custom workflows:

```python
import sys
sys.path.insert(0, "scripts")  # relative to skill root directory
from binance_square_monitor import fetch_48h_full, fetch_trending_posts

# Fetch full 48-hour data
posts, stats = fetch_48h_full(hours=48, page_size=20, verbose=True)
print(f"Total posts collected: {len(posts)}")

# Fetch single page of trending posts
trending = fetch_trending_posts(page_index=1, page_size=20)
```

## API Details

For endpoint documentation, parameters, and response schema, see `references/api_reference.md`.

The 48-hour full fetch utilizes multiple endpoints to ensure comprehensive coverage:
1. `GET /bapi/composite/v3/friendly/pgc/content/article/list?type=2` (Latest posts)
2. `GET /bapi/composite/v4/friendly/pgc/feed/news/list` (News feed)
3. `GET /bapi/composite/v3/friendly/pgc/content/article/list?type=1` (Trending posts)

## Troubleshooting

**Empty results**: Binance may block requests from certain IPs. Retry after a few minutes or adjust User-Agent header in the script.

**Encoding errors**: The API returns gzip-compressed responses. The `requests` library handles this automatically; `curl` requires `--compressed` flag.
