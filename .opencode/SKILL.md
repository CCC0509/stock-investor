# Stock Investor 專案 Skill

## 專案基本資訊

- **專案名稱**: stock-investor
- **Repo**: https://github.com/CCC0509/stock-investor
- **用途**: 股票投資輔助網站，朝向自動化投資發展

## 技術架構

- **前端**: Next.js 14+ (App Router) + TypeScript + Tailwind CSS + shadcn/ui
- **後端**: NestJS + TypeScript
- **資料庫**: PostgreSQL（部署於 Raspberry Pi 5）
- **目錄結構**: Monorepo（apps/web + apps/api + packages/shared）

## 核心功能模組

1. **股票資料模組** - 報價、K線圖、基本資料
2. **投資組合模組** - 持股追蹤、損益計算
3. **自動化交易模組** - 條件單、策略回測
4. **價格提醒模組** - 目標價格通知
5. **新聞分析模組** - AI 新聞情緒分析、股票推薦

## 重要約定

- **語言**: 所有回覆與 Markdown 文件皆使用繁體中文
- **設計文件**: `docs/superpowers/specs/2026-04-03-stock-investor-design.md`
- **專案記憶**: `MEMORY.md`

## 開發階段

- Phase 1: MVP（專案初始化、股票查詢、投資組合）
- Phase 2: 核心功能（即時報價、提醒、交易記錄）
- Phase 3: AI 功能（新聞爬蟲、AI 分析、推薦系統）
- Phase 4: 自動化（條件單、回測、通知）

## 工作流程

1. 開發前先閱讀 `MEMORY.md` 確認專案狀態
2. 查看 `docs/superpowers/specs/` 中的設計規格
3. 遵循 Monorepo 目錄結構
4. 完成後同步更新 MEMORY.md
5. 重要變更需 commit 並 push

## 部署位置

| 元件 | 位置 |
|------|------|
| 前端 | Vercel |
| 後端 API | Raspberry Pi 5 |
| 資料庫 | Raspberry Pi 5 (PostgreSQL) |
