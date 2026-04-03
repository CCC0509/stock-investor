# 股票投資系統設計規格書

## 專案概述

- **專案名稱**: stock-investor
- **建立時間**: 2026-04-03
- **類型**: 全端網頁應用
- **目標用戶**: 個人投資者及親友
- **願景**: 成為自動化投資決策輔助系統

---

## 技術架構

### 技術堆疊

| 層級 | 技術 | 說明 |
|------|------|------|
| 前端 | Next.js 14+ (App Router) | React 元件庫 + TypeScript |
| 後端 | NestJS | Node.js 框架 + TypeScript |
| 資料庫 | PostgreSQL | 部署於 Raspberry Pi |
| 即時資料 | WebSocket | 股票報價即時更新 |
| 新聞分析 | AI API | OpenAI / Claude API |

### 目錄結構（Monorepo）

```
stock-investor/
├── apps/
│   ├── web/              # Next.js 前端
│   │   ├── app/
│   │   ├── components/
│   │   └── lib/
│   └── api/              # NestJS 後端
│       ├── src/
│       │   ├── modules/
│       │   ├── services/
│       │   └── entities/
│       └── test/
├── packages/
│   └── shared/           # 共用 types、utils
├── docs/
│   └── superpowers/
│       └── specs/        # 設計規格文件
├── docker-compose.yml    # 本地開發用
├── MEMORY.md             # 專案記憶
└── README.md
```

### 主機部署

| 元件 | 位置 | 備註 |
|------|------|------|
| 前端 | Vercel | 免費、CDN、自动部署 |
| 後端 API | Raspberry Pi 5 | 自架、反向代理 |
| 資料庫 | Raspberry Pi 5 | PostgreSQL |

---

## 功能模組

### 1. 股票資料模組

**功能：**
- 股票搜尋（代碼、名稱）
- 即時報價顯示
- 互動式 K 線圖
- 基本資料（股本、營收、EPS）

**資料來源：**
- TWSE（台灣證券交易所）
- Yahoo Finance API
- Finnhub API

**K線圖技術：**
- TradingView Lightweight Charts（開源免費）

---

### 2. 投資組合模組

**功能：**
- 新增/編輯/刪除持股記錄
- 持有成本計算
- 當前市值（串接即時報價）
- 報酬率計算（單筆/整體）
- 歷史交易記錄

**資料模型：**

```typescript
interface Portfolio {
  id: string;
  name: string;
  holdings: Holding[];
}

interface Holding {
  stockCode: string;
  shares: number;
  avgCost: number;       // 平均成本
  buyDate: Date;
}

interface Transaction {
  id: string;
  stockCode: string;
  type: 'BUY' | 'SELL';
  shares: number;
  price: number;
  date: Date;
  portfolioId: string;
}
```

---

### 3. 自動化交易模組

**功能：**
- 條件單設定（價格/漲跌%）
- 策略回測（基於歷史資料）
- 通知觸發（未實際下單）

**安全考量：**
- 初期僅作提醒，不實際執行交易
- 未來串接券商 API 需額外安全機制

---

### 4. 價格提醒模組

**功能：**
- 設定目標價格（上限/下限）
- 電子郵件/LINE 通知
- 網頁推播

**實現方式：**
- 後端 Cron Job 每分鐘檢查
- 觸發時發送通知

---

### 5. 新聞分析模組（核心特色）

**功能：**
- 爬蟲抓取全球財經新聞
- AI 分析新聞情緒（看漲/看跌）
- 根據分析推薦關注股票
- 新聞與股價關聯性分析

**新聞來源：**
- Yahoo Finance News
- Bloomberg RSS
- 鉅亨網

**AI 分析流程：**
```
新聞文字 → 清理/摘要 → AI API → 情緒分析結果 → 推薦股票
```

**推薦邏輯：**
- 正面新聞多 → 推薦關注
- 負面新聞多 → 警示風險
- 顯示相關性分數

---

## 使用者介面設計

### 主要頁面

1. **儀表板（Dashboard）**
   - 今日關注股票
   - 投資組合總覽
   - 最新新聞推薦

2. **股票查詢（Stock）**
   - 搜尋列
   - K線圖
   - 基本資料
   - 相關新聞

3. **投資組合（Portfolio）**
   - 持股列表
   - 績效圖表
   - 交易記錄

4. **新聞中心（News）**
   - 新聞列表
   - AI 分析結果
   - 推薦股票卡片

5. **設定（Settings）**
   - 提醒設定
   - API Key 管理
   - 主題切換

### UI 風格

- **設計系統**: Tailwind CSS + shadcn/ui
- **色調**: 專業金融風格（深色/淺色模式）
- **圖表**: 統一一致的数据視覺化

---

## API 設計

### REST API

```
GET    /api/stocks/:code          # 取得股票資料
GET    /api/stocks/:code/quote    # 取得即時報價
GET    /api/stocks/:code/chart    # 取得K線資料

GET    /api/portfolios            # 取得投資組合列表
POST   /api/portfolios            # 建立投資組合
GET    /api/portfolios/:id        # 取得單一組合
PUT    /api/portfolios/:id        # 更新組合
DELETE /api/portfolios/:id        # 刪除組合

POST   /api/transactions          # 新增交易記錄
GET    /api/transactions          # 取得交易記錄

GET    /api/news                  # 取得新聞列表
GET    /api/news/analyze/:code    # 分析特定股票新聞
```

### WebSocket

```
/ws/stocks                      # 即時股票報價
```

---

## 安全性考量

1. **認證**：JWT Token
2. **API 安全**：Rate Limiting、Input Validation
3. **敏感資料**：加密儲存於 Raspberry Pi
4. **備份**：定時快照到雲端儲存

---

## 開發階段

### Phase 1 - MVP（2-4週）
- [ ] 專案初始化（Monorepo）
- [ ] 股票查詢功能
- [ ] 基本 K 線圖
- [ ] 投資組合 CRUD

### Phase 2 - 核心功能（4-6週）
- [ ] 即時報價（WebSocket）
- [ ] 價格提醒
- [ ] 交易記錄

### Phase 3 - AI 功能（6-8週）
- [ ] 新聞爬蟲
- [ ] AI 分析整合
- [ ] 推薦系統

### Phase 4 - 自動化（8-10週）
- [ ] 條件單邏輯
- [ ] 回測系統
- [ ] 通知系統

---

## 待確認事項

- [ ] 特定券商 API 需求（若有實際下單需求）
- [ ] 用戶數量上限評估
- [ ] 備份策略細節

---

*最後更新：2026-04-03*
