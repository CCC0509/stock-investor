# 股票投資系統 - 專案記憶

## 基本資訊
- **專案名稱**: stock-investor
- **建立時間**: 2026-04-03
- **Repo**: https://github.com/CCC0509/stock-investor

## 專案概述
網頁與 App 前後端開發的總管理系統的子專案，目標是建立一個股票投資輔助網站，最終朝向自動化投資方向發展。

## 技術架構
- **前端**: Next.js 14+ (App Router) + TypeScript
- **後端**: NestJS + TypeScript
- **資料庫**: PostgreSQL（部署於 Raspberry Pi 5）
- **部署**: Vercel（前）+ Raspberry Pi（後）

## 核心功能
1. 股票資料查詢（報價、K線圖、基本資料）
2. 投資組合追蹤（記錄買賣、計算損益）
3. 自動化交易（條件單、策略回測）
4. 價格提醒通知
5. 全球新聞分析與股票推薦（AI）

## 開發階段
- Phase 1: MVP（專案初始化、股票查詢、投資組合）
- Phase 2: 核心功能（即時報價、提醒、交易記錄）
- Phase 3: AI 功能（新聞爬蟲、AI 分析、推薦系統）
- Phase 4: 自動化（條件單、回測、通知）

## 設計文件
- 規格書：`docs/superpowers/specs/2026-04-03-stock-investor-design.md`

## 重要約定
- 語言：所有回覆使用繁體中文
- 架構：Monorepo（apps/web + apps/api + packages/shared）
