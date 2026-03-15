# 多終端協作規則

> 這個 repo 有多個 Claude Code session 同時在改。
> 為了不互相覆蓋和刪檔案，請遵守以下規則。

## 分工

| 終端 | 負責範圍 | 不要碰 |
|------|----------|--------|
| **終端 A**（主力） | 即時引擎、學習、進化、斷路器、MCP、chatbot | analysis_*.py |
| **終端 B**（修復） | FIXSPEC 的代碼修復、analysis_*.py、utils.py | realtime_loop.py、learning_engine.py |
| **終端 C**（分析） | X vs TS 分析、新數據源 | 核心引擎檔案 |

## 共用檔案（改之前先溝通）

這些檔案多個終端都會碰，改之前要先 `git pull`：
- `daily_pipeline.py` — 主管線
- `data/surviving_rules.json` — 規則庫
- `data/signal_confidence.json` — 信號信心度
- `README.md`

## 絕對禁止

1. **`git push --force`** — 這會刪掉別人的 commit。已經發生 3 次了
2. **不 pull 就 push** — 一定先 `git pull --rebase` 再 push
3. **大規模改別人負責的檔案** — 先溝通

## 衝突處理 SOP

```bash
# 1. 先 pull
git pull --rebase origin main

# 2. 如果有衝突
git status  # 看哪些檔案衝突

# 3. 手動解決衝突（保留兩邊的改動）
# 4.
git add <衝突的檔案>
git rebase --continue
git push origin main
```

## 檔案所有權

```
終端 A 的檔案（別人別碰）：
  realtime_loop.py, learning_engine.py, rule_evolver.py,
  circuit_breaker.py, mcp_server.py, chatbot_server.py,
  polymarket_client.py, kalshi_client.py, arbitrage_engine.py,
  signal_market_mapper.py, pm_feedback_loop.py, event_detector.py,
  dual_platform_signal.py, ai_signal_agent.py, trump_code_cli.py

終端 B 的檔案（別人別碰）：
  analysis_01~12_*.py, utils.py, clean_data.py, FIXSPEC.md

終端 C 的檔案（別人別碰）：
  x_*.py, analyze_x_*.py, multi_source_fetcher.py,
  build_own_archive.py, deletion_detector.py

共用（改之前 pull）：
  daily_pipeline.py, trump_monitor.py, README.md, data/
```
