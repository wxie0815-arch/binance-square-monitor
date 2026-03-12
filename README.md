# 📈 Binance Square Monitor — 币安广场热帖监控 Skill

[![Version](https://img.shields.io/badge/version-2.1.0-blue)](https://github.com/wxie0815-arch/binance-square-monitor)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Compatible-green)](https://openclaw.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> 实时抓取并监控币安广场的热门帖子，追踪浏览量、点赞、评论、分享等流量数据。支持单次快照、连续监控和48小时全量历史数据抓取。

## 🎯 功能概述

`binance-square-monitor` 是一个独立的监控工具，同时也是 `binance-square-oracle` 的核心数据来源（L0层）。它提供强大的数据抓取能力，为市场分析和内容策略提供数据支持。

## ✨ 核心特性

- **48小时全量数据**：通过多API源融合去重，抓取过去48小时的全部帖子数据。
- **实时监控**：支持按指定时间间隔连续抓取，并记录流量变化增量。
- **多格式输出**：支持将数据保存为 CSV、JSON，并生成 Markdown 格式的分析报告。
- **无需认证**：所有抓取均在未登录状态下通过公开API完成，无需任何凭证。
- **灵活的命令行接口**：提供 `fetch`, `fetch-48h`, `monitor` 三种命令，满足不同场景需求。

## 🚀 快速开始

### 安装

```bash
gh repo clone wxie0815-arch/binance-square-monitor
cd binance-square-monitor
```

### 命令行使用

```bash
# 抓取过去48小时全量数据并生成报告
python3 scripts/binance_square_monitor.py fetch-48h --output ./data

# 抓取最新的3页热门帖子
python3 scripts/binance_square_monitor.py fetch --pages 3 --output ./data

# 每5分钟监控一次，持续运行
python3 scripts/binance_square_monitor.py monitor --interval 300 --output ./data
```

### 作为模块使用

```python
import sys
sys.path.insert(0, "scripts")
from binance_square_monitor import fetch_48h_full, fetch_trending_posts

# 获取48小时全量数据
posts, stats = fetch_48h_full()

# 获取最新一页热门帖子
trending = fetch_trending_posts(page_index=1)
```

详细的命令行参数和输出文件说明，请参考 `SKILL.md`。

## 🔗 相关 Skill

| Skill | 说明 | 仓库 |
|-------|------|------|
| `binance-square-oracle` | 集成此监控功能的完整预言机 | [wxie0815-arch/binance-square-oracle](https://github.com/wxie0815-arch/binance-square-oracle) |

## 📄 许可证

MIT License

---

## 💰 赞助支持

如果这个项目对您有帮助，欢迎赞助支持！

**BSC（BEP-20）钱包地址：**
`0x3B74BE938caB987120C3661C8e3161CD838e5a1A` 

支持 USDT / BNB / 任意 BEP-20 代币。感谢每一位支持者 🙏

**作者：** 无邪Infinity | 币安广场 [@wuxie](https://www.binance.com/en/square/profile/wuxie) | X [@wuxie149](https://x.com/wuxie149)
