# Binance Square Monitor

A powerful skill for monitoring Binance Square trending posts and tracking real-time traffic data including view count, like count, comment count, and share count.

## Features

- **48-Hour Full Data Fetch**: Scrape all posts from the past 48 hours using multiple data sources with automatic deduplication
- **Real-time Monitoring**: Continuous monitoring with change tracking
- **Comprehensive Reports**: Generate detailed reports and analytics
- **Multiple Output Formats**: CSV, JSON, and terminal display
- **Change Tracking**: Monitor traffic changes over time

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/wxie0815-arch/binance-square-monitor.git
cd binance-square-monitor

# Install dependencies (only requests needed)
pip install requests
```

### 48-Hour Full Data Fetch

Scrape all posts from the past 48 hours:

```bash
python3 scripts/binance_square_monitor.py fetch-48h --output ./data
```

### Single Fetch (One-time Snapshot)

```bash
python3 scripts/binance_square_monitor.py fetch --pages 3 --output ./data
```

### Continuous Monitoring

```bash
python3 scripts/binance_square_monitor.py monitor --interval 300 --pages 3 --output ./data
```

## Data Fields Collected

Each post record contains:
- `post_id`: Unique post identifier
- `author`: Author display name
- `view_count`: Total views
- `like_count`: Total likes
- `comment_count`: Total comments
- `share_count`: Total shares
- `reply_count`: Total replies
- `quote_count`: Total quotes
- `post_time`: Post creation time (UTC)
- `hashtags`: Associated hashtags
- `web_link`: Direct URL to post

## Output Files

- `48h_full_data.csv`: Full dataset from 48-hour fetch
- `48h_full_data.json`: Structured JSON with stats
- `48h_report.md`: Comprehensive summary report
- `trending_posts.csv`: All snapshots with full traffic data
- `trending_posts.json`: Structured JSON with snapshot grouping
- `changes_log.csv`: Delta values between consecutive snapshots
- `monitor_report.md`: Summary report (generated on exit for monitor)

## Programmatic Usage

```python
import sys
sys.path.insert(0, "/path/to/binance-square-monitor/scripts")
from binance_square_monitor import fetch_48h_full, fetch_trending_posts

# Fetch full 48-hour data
posts, stats = fetch_48h_full(hours=48, page_size=20, verbose=True)
print(f"Total posts collected: {len(posts)}")

# Fetch single page of trending posts
trending = fetch_trending_posts(page_index=1, page_size=20)
```

## License

MIT

## Author

芒果 (Mango) - AI Assistant for 无邪 (Web3博主+交易员)