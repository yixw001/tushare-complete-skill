---
name: "tushare-complete"
description: "Tushare Pro完整接口库（266个API），用于获取A股/港股/美股行情、财务报表、资金流向、基金净值、期货期权、债券、宏观经济指标（GDP/CPI/PMI/Shibor）。Use when fetching Chinese financial market data, stock prices, financial statements, fund NAV, futures, bonds, or macro indicators via Tushare."
---

# Tushare Complete - 完整接口库

通过Tushare Pro API获取A股、港股、美股、基金、期货、债券、宏观经济等金融数据。覆盖266个接口，详见 [reference/](reference/README.md)。

## Workflow

### 1. Initialize Connection

```python
import tushare as ts
pro = ts.pro_api('your_token_here')
```

> **Token**: Set via `ts.set_token('your_token')` or environment variable `TUSHARE_TOKEN`. Get a token at https://tushare.pro/register

### 2. Fetch Data

**Stock prices:**
```python
# Single stock daily history
df = pro.daily(ts_code='000001.SZ', start_date='20240101', end_date='20241231')

# Multiple stocks
df = pro.daily(ts_code='000001.SZ,600000.SH', start_date='20240101', end_date='20241231')

# All stocks for one day
df = pro.daily(trade_date='20241231')
```

**Financial statements:**
```python
# Balance sheet (includes cip=在建工程, fix_assets=固定资产)
df = pro.balancesheet(ts_code='600000.SH', start_date='20240101', end_date='20241231')

# Cash flow (c_pay_acq_const_fiolta=资本开支, n_cashflow_act=经营现金流)
df = pro.cashflow(ts_code='600000.SH', start_date='20240101', end_date='20241231')

# Income statement
df = pro.income(ts_code='600000.SH', start_date='20240101', end_date='20241231')

# Financial indicators (roe, roa, net_profit_margin, debt_to_assets)
df = pro.fina_indicator(ts_code='600000.SH', start_date='20240101', end_date='20241231')
```

**Macro indicators:**
```python
df = pro.gdp(start_q='20220101', end_q='20221231')   # GDP
df = pro.cpi(start_date='20220101', end_date='20221231')  # CPI
df = pro.pmi(start_date='20220101', end_date='20221231')  # PMI
df = pro.shibor(start_date='20240101', end_date='20240131')  # Shibor
df = pro.lpr(start_date='20240101', end_date='20240131')   # LPR
```

**Hong Kong / US stocks:**
```python
df = pro.hk_daily(ts_code='00700.HK', start_date='20240101', end_date='20241231')
df = pro.us_daily(ts_code='AAPL', start_date='20240101', end_date='20241231')
```

### 3. Handle Errors

```python
# Rate limit: max 120 calls/min (free), higher with credits
import time

def fetch_with_retry(func, max_retries=3, **kwargs):
    for attempt in range(max_retries):
        try:
            df = func(**kwargs)
            if df is None or df.empty:
                print(f"Empty response for {kwargs}")
                return None
            return df
        except Exception as e:
            if '每分钟' in str(e) or 'freq' in str(e):
                time.sleep(15)
            elif '权限' in str(e) or 'credits' in str(e):
                print(f"Insufficient credits: {e}")
                return None
            else:
                raise
    return None
```

**Common issues:**
- Empty DataFrame → check `ts_code` format (e.g., `000001.SZ` not `000001`) and date range
- 权限不足 → some interfaces require credits (基础: free, 财务: 2000, 高级: 5000). See https://tushare.pro/document/1?doc_id=108
- Rate limiting → add `time.sleep(0.5)` between calls when batch-fetching

## Interface Categories

| Category | Count | Key Interfaces | Details |
|----------|-------|----------------|---------|
| 股票基础 | 14 | `stock_basic`, `daily_basic`, `trade_calendar` | [reference/接口文档/](reference/接口文档/) |
| 行情数据 | 23 | `daily`, `weekly`, `monthly`, `stk_mins` | [reference/接口文档/](reference/接口文档/) |
| 财务数据 | 10 | `income`, `balancesheet`, `cashflow`, `fina_indicator` | [reference/FIELD_REFERENCE.md](reference/FIELD_REFERENCE.md) |
| 基金数据 | 15+ | `fund_basic`, `fund_nav`, `fund_daily` | [reference/接口文档/](reference/接口文档/) |
| 期货期权 | 20+ | `fut_basic`, `fut_daily`, `opt_basic`, `opt_daily` | [reference/接口文档/](reference/接口文档/) |
| 宏观经济 | 28 | `gdp`, `cpi`, `pmi`, `shibor`, `lpr` | [reference/接口文档/](reference/接口文档/) |
| 港股美股 | 10+ | `hk_daily`, `us_daily`, `hk_basic`, `us_basic` | [reference/接口文档/](reference/接口文档/) |

Full field reference: [reference/FIELD_REFERENCE.md](reference/FIELD_REFERENCE.md)
Quick code examples: [QUICK_REFERENCE.md](QUICK_REFERENCE.md)

## Credits & Limits

- Free tier: 120 calls/min, basic stock data
- 2000 credits: financial statements, fund data
- 5000 credits: advanced data, higher frequency

Official docs: https://tushare.pro/document/2
