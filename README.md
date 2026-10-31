# 🌸 三麗鷗可愛計畫本

給國小三年級使用的單檔互動週計畫表，粉紅三麗鷗風格。整份網站就是 `index.html`（HTML + CSS + JavaScript 全部內嵌，無需建置）。

## 線上使用

GitHub Pages 發布後可直接用手機或電腦開啟：

```
https://workdaf0917-cmd.github.io/study-planner/
```

## 功能

- **週計畫表**：週一～週日、00:00–23:30 半小時時間格，可新增、完成、刪除任務，拖曳任務換時段
- **週次顯示**：週計畫模式會標示「幾月的第幾週 ＋ 日期區間」（例如 `10月第5週 10/26 – 11/1`），星期分頁上也會標日期；一週歸屬哪一個月以該週星期四所在月份為準
- **常用任務選單**：新增任務時可直接點選三年級的起居／課業／才藝／飲食模板，也能「一次加入這一組」（同名任務會自動跳過）
- **懸浮球工具列**：新增任務、每日復盤、匯出紀錄、匯入紀錄固定在畫面下方，跟著頁面捲動，可按 ▾ 收成一個小圓球
- **萬年曆**：點選任意日期，切換成單日模式安排當天任務
- **每日復盤**：寫下今天的心得並獲得積分
- **匯出／匯入 JSON**：備份或轉移資料到其他裝置，採固定格式（見下方）

## 資料儲存

所有資料存在瀏覽器的 `localStorage`（key：`sanrio_plan_data`），不會上傳到任何伺服器。
介面偏好（懸浮球收合與否）另存 `sanrio_plan_ui_pref`，不會混進備份檔。
換裝置或清除瀏覽器資料前，請先用「匯出」備份，再到新裝置「匯入」。

帶有 `date`（`YYYY-MM-DD`）的任務是單日任務；沒有 `date` 的任務是每週重複的例行任務，其 `doneDates` 記錄「在哪幾天完成過」，所以在某個日期打勾不會影響其他週。

## 匯出／匯入格式（`sanrio-plan` v2）

匯出檔名：`sanrio-plan-v2-YYYY-MM-DD.json`。外層用 `format` + `schemaVersion` 標記格式，實際資料只在 `data` 裡面；`summary`、`exportedAt`、`help` 等欄位匯入時會被忽略。

```json
{
  "format": "sanrio-plan",
  "schemaVersion": 2,
  "app": "三麗鷗可愛計畫本",
  "exportedAt": "2026-10-31T12:00:00.000Z",
  "help": "固定格式說明（匯入時忽略）",
  "summary": { "taskCount": 12, "weeklyCount": 10, "datedCount": 2 },
  "data": {
    "tasks": [
      {
        "id": "task-m3k2x1-1-a7b9",
        "day": "週一",
        "date": null,
        "time": "17:00",
        "title": "寫功課",
        "points": 15,
        "completed": false,
        "icon": "📚",
        "order": 0,
        "doneDates": ["2026-10-26"]
      }
    ],
    "reviewBonuses": { "週一": 20, "2026-11-04": 50 }
  }
}
```

任務欄位固定為上列 10 個：`id`、`day`（`週一`～`週日`）、`date`（`YYYY-MM-DD` 或 `null`）、`time`（`00:00`～`23:30` 的半小時）、`title`、`points`（1–100）、`completed`、`icon`、`order`、`doneDates`。`reviewBonuses` 的索引是星期名或日期字串，值只能是 `20` 或 `50`。

匯入的相容規則（介面改版後舊檔仍可用）：

- 沒有 `format` 欄位的舊檔一律視為 v1，自動轉換（`星期一`／`禮拜一`／`周1` 都會換成 `週一`，數字 `id` 換成字串，缺 `date` 的任務當成每週任務）
- 看不懂的欄位忽略、缺少的欄位補預設值，格式不符的任務會被略過並回報數量
- `schemaVersion` 比目前版本新時會先提示，仍盡量匯入
- 匯入前會顯示檔案內容摘要並要求確認，匯入後維持目前正在看的那一天／那一週

未來若需要破壞性調整，請把 `SCHEMA_VERSION` 加 1，並在 `readImportPayload()`／`normalizeState()` 補上對應的轉換，不要改動 v2 已定義的欄位語意。

## 本機預覽

直接用瀏覽器打開 `index.html` 即可，不需要伺服器。
注意 `localStorage` 綁網域：本機檔案與 GitHub Pages 網址是兩份獨立的資料，跨來源請用匯出／匯入搬移。
