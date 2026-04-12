# stock-analysis

# 台灣股市 AI 分析專案

## 專案介紹
本專案利用 Python 與 AI 工具，蒐集台灣股市資料，並分析半導體與 AI 類股。

---

## 資料來源
- yfinance API

---

## 類股分類

### 半導體
- 2330 台積電
- 2303 聯電
- 2454 聯發科

### AI 類股
- 2382 廣達
- 2356 英業達
- 6669 緯穎

---

## K線圖分析

### 半導體族群
- 長期上升趨勢
- 波動較穩定

### AI 類股
- 波動較大
- 短期漲跌明顯

---

## 使用技術
- Python
- yfinance
- mplfinance

---

## 成果展示（程式碼）

import yfinance as yf
import pandas as pd

# 台股股票（要加 .TW）
stocks = {
    "半導體": ["2330.TW", "2317.TW", "2303.TW", "2454.TW", "3034.TW"],
    "AI": ["2382.TW", "2356.TW", "2376.TW", "6669.TW", "3231.TW"]
}

data = {}

for category, tickers in stocks.items():
    data[category] = {}
    for ticker in tickers:
        df = yf.download(ticker, start="2024-01-01", end="2024-12-31")
        df = df[['Open', 'High', 'Low', 'Close']]
        data[category][ticker] = df

print("資料抓取完成")

————————————————————————————

!pip install mplfinance

————————————————————————————

import mplfinance as mpf
import pandas as pd

for category in data:
    for ticker in data[category]:
        df = data[category][ticker].copy()

        # 🔥 如果是 multi-index（保險寫法）
        if isinstance(df.columns, pd.MultiIndex):
            df.columns = df.columns.get_level_values(0)

        # 修正 index
        df.index = pd.to_datetime(df.index)

        # 保留欄位
        df = df[['Open', 'High', 'Low', 'Close']]

        # 轉數字
        df = df.apply(pd.to_numeric, errors='coerce')

        # 清理
        df = df.dropna()
        df = df.sort_index()

        mpf.plot(
            df,
            type='candle',
            title=f"{category} - {ticker}",
            style='charles',
            volume=False
        )
