# CQshare · 观潮量化行情源接口文档

> **CQshare** —— 观潮量化（GUANCHAO QUANT）对外行情数据接口文档仓库。
> 只放文档，不放代码。站点：<https://quant.tail6a6cc8.ts.net/>

面向开发者：用 HTTP API 或本地 SQLite 直连接入观潮量化的行情源，
覆盖 **A股 / 港股 / 美股 / 加密货币 / 国内期货** 五个市场的日频行情。

## 快速开始

```bash
# 单标行情
curl "https://quant.tail6a6cc8.ts.net/api/quote?code=sh600519"

# K线（日/周/月）
curl "https://quant.tail6a6cc8.ts.net/api/kline?code=us:AAPL&period=week&tail=52"

# K线 + 指标（MA/MACD/RSI/KDJ/BOLL）
curl "https://quant.tail6a6cc8.ts.net/api/indicators?code=crypto:BTC/USDT&period=day&tail=300"
```

```python
import requests

BASE = "https://quant.tail6a6cc8.ts.net"
d = requests.get(BASE + "/api/indicators",
                 params={"code": "sh600519", "period": "day", "tail": 300},
                 timeout=10).json()

bars = d["bars"]                    # OHLCV + 指标列
ma5 = d["indicators"]["MA"]["MA5"]  # 与 bars 对齐的序列
```

## 接口一览

| 接口 | 说明 |
|---|---|
| `GET /api/markets` | 五市场看板行情（60 秒缓存） |
| `GET /api/quote?code=xxx` | 单标概览 + 最近 30 个交易日 |
| `GET /api/kline?code=xxx&period=day\|week\|month&tail=250` | 纯 K 线 |
| `GET /api/indicators?code=xxx&period=day&tail=300` | K 线 + 指标序列 |
| `GET /api/search?q=xxx` | 代码/名称搜索 |
| `GET /health` | 健康检查 |

## 编码规则

| 市场 | 格式 | 示例 |
|---|---|---|
| A股 | `sh/sz + 6位代码` | `sh600519` 贵州茅台 |
| A股指数 | `sh000xxx` / `sz399xxx` | `sh000001` 上证指数 |
| 港股 | `hk:5位补零` | `hk:00700` 腾讯控股 |
| 美股 | `us:Ticker` | `us:AAPL` |
| 加密货币 | `crypto:BASE/QUOTE` | `crypto:BTC/USDT` |
| 期货 | `fut:品种+0` | `fut:IH0` 上证50 |

## 完整文档

- [docs/API.md](docs/API.md) —— 接口明细、字段说明、指标口径、本地数据库直连、注意事项
- 在线版：<https://quant.tail6a6cc8.ts.net/docs>

## 联系

- 维护者：獨白
- Telegram：[@Monologue_101](https://t.me/Monologue_101)
- QQ：2180287399

---

*接口仅开放给观潮量化 Tailscale 网络内用户，仅供只读研究与学习使用，不构成投资建议。*
