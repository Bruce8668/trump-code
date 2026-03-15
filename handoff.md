# 交接文件
> 日期：2026-03-15 | 摘要：川普密碼從開源分析工具升級成完整的 AI 閉環預測系統

## 身份
你是 tkman 的 AI CTO。這個 session 做了 30+ commits，把 trump-code 從靜態分析升級成即時預測+自動學習+預測市場套利的完整系統。

## 系統一句話
兩個引擎並行：`realtime_loop.py`（每 5 分鐘即時偵測+雙軌追蹤 PM+美股）+ `daily_pipeline.py`（每天學習+進化+報告）。Opus 當大腦，Gemini Flash 當嘴巴。斷路器防全錯。

## 已完成（30+ commits）

### 審查 + 修復
- [x] 4 AI 交叉審查 48 個問題 → FIXSPEC.md
- [x] C1 模型加情緒過濾（attack > positive 不觸發）
- [x] 信號信心度調整（DEAL↓0.55 RELIEF↑0.85 THREAT↑0.60）
- [x] Gemini key 從代碼移到環境變數
- [x] 被另一邊 force-push 刪掉的 3 檔案已恢復

### 預測市場
- [x] polymarket_client.py — 修好搜尋（slug_contains 多關鍵字）
- [x] kalshi_client.py — 公開 API，跨平台價差偵測
- [x] arbitrage_engine.py + signal_market_mapper.py — 信號→套利分數
- [x] pm_feedback_loop.py — 追蹤 PM 結果→自動調信號信心度

### 閉環學習
- [x] learning_engine.py — 升級/降級/淘汰（D3⬆️ C3🗑️）
- [x] rule_evolver.py — 交配/突變/精煉（每次跑 +50 條新規則）
- [x] circuit_breaker.py — 4 道防線 + 從錯誤中學（排除法）

### 即時引擎（最重要）
- [x] realtime_loop.py — 每 5 分鐘偵測→分類→雙軌追蹤（PM+SPY）
- [x] 事件分級（EVENT ×10 / NOTABLE ×3 / NOISE ×0）
- [x] event_detector.py — 多日醞釀模式（關稅轟炸、RELIEF 轉折、爆量沉默）
- [x] dual_platform_signal.py — TS vs X（中國 ×1.5、6h 窗口、X 降權 ×0.8）

### AI Agent
- [x] ai_signal_agent.py — Opus 簡報包→分析→寫回
- [x] Opus 第一次分析完成

### 對外介面
- [x] mcp_server.py — 9 個 MCP 工具
- [x] trump_code_cli.py — CLI 8 指令
- [x] chatbot_server.py — Gemini Flash×3 key + 群眾智慧 + 每日 500 次額度
- [x] README 全面更新

## 關鍵數據發現

| 發現 | 怎麼用 |
|------|--------|
| RELIEF 信號 → +9.52% | 信心度 0.85，A3 最強模型 |
| TARIFF→SHORT 70% 錯 | 自動降權，別再看到關稅就做空 |
| 中國信號 0% 放 X | 加權 ×1.5（刻意隱藏=更真實）|
| TS→X 延遲 6.2h | 窗口內做多（63% 偏漲）|
| 大事前 3 天關稅 2.7 倍 | 事件偵測器監控醞釀 |
| 系統正在惡化（50% vs 61%） | 斷路器持續監控 |

## 全測試結果
18/18 模組 import ✅ | CLI ✅ | MCP ✅ | Polymarket ✅ | 學習 ✅ | 即時 ✅

## 已知問題
- 另一邊同時改 repo，經常 conflict + force-push 刪檔案（已恢復 3 次）
- Polymarket 搜尋找到的主要是 GTA VI 相關市場，真正的 tariff/trade 市場 slug 不同
- Kalshi 目前 0 個 Trump 政治市場
- 美股數據(yfinance)盤外時段可能回 None
- chatbot 需要 `export GEMINI_KEYS=` 環境變數

## tkman 的核心指導（要記住的）
1. **預測市場 = 美股，同時都要算**，不是替代。差異就是套利
2. **只學大事**，雞毛蒜皮不理。EVENT ×10 / NOISE ×0
3. **看前幾天的推文**，不是只看一篇。大資金需要運作時間
4. **全錯了也是數據**。排除法：做錯做錯做錯→對的方向就出來
5. **Opus 用本機的**，不用 API key
6. **別人的邏輯也要收**。聊天機器人收群眾智慧
7. **不能秀個人資料**。匿名、key 走環境變數

## 下一步

### 🔴 最重要
1. 跟另一邊協調 — 他們不能再 force-push 刪我們的檔案
2. 把美股即時數據接好 — 盤中用 SPY 1 分鐘數據，盤外用 ES 期貨
3. 定義更精確的「事件」— 不只看漲跌幅，看有沒有改變市場共識方向

### 🟡 中優先
4. 部署 chatbot 到 VPS（等 tkman 說「部署」）
5. 找到 Polymarket 上真正的 Trump tariff/trade 市場 slug
6. 群眾洞見公開頁面
7. 把 Opus 分析的新規則假設自動加入規則庫

### 🟢 低優先
8. 更多進化策略（目前只有 2/3 條件，可以試 4 條件）
9. 接更多數據源（Bitcoin、Gold、Oil、VIX 與信號的交叉）
10. 反向信號模型（C3 反著做 62% 對→做成正式模型）

## 檔案地圖
```
/tmp/trump-code/
├── 即時引擎
│   ├── realtime_loop.py        每 5 分鐘偵測+雙軌追蹤
│   ├── event_detector.py       多日醞釀模式
│   └── dual_platform_signal.py TS vs X 雙平台加權
├── 每日管線
│   ├── daily_pipeline.py       11 步管線
│   └── trump_monitor.py        11 個命名模型
├── 學習層
│   ├── learning_engine.py      升級/降級/淘汰
│   ├── rule_evolver.py         交配/突變/精煉
│   ├── circuit_breaker.py      斷路器+排除學習
│   └── pm_feedback_loop.py     PM 結果→回饋
├── 預測市場
│   ├── polymarket_client.py    Polymarket API
│   ├── kalshi_client.py        Kalshi API
│   ├── arbitrage_engine.py     套利引擎
│   └── signal_market_mapper.py 信號→市場映射
├── AI 層
│   └── ai_signal_agent.py      Opus 簡報包
├── 對外
│   ├── mcp_server.py           9 個 MCP 工具
│   ├── trump_code_cli.py       CLI 8 指令
│   └── chatbot_server.py       聊天機器人
├── 分析（12 個）
│   └── analysis_01~12_*.py
└── data/（48 個數據檔案）
```

## GitHub
https://github.com/sstklen/trump-code (public)
40 個 Python 檔案 | 16,903 行 | 550 條規則 | 564 筆驗證 | 61.3% 命中率
