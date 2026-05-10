# 保障健診 PWA

把保障彙整表 PDF 自動解析,對應到「生涯健診表」欄位,並可一鍵匯出成 PNG 圖檔分享給保戶的 iPhone PWA。

## 功能

- 📤 上傳保障彙整表 PDF
- 🤖 兩種解析模式:
  - **手動模式 (預設,免費)**:複製提示詞 → 用你的 Claude Pro 解析 → 貼回 JSON
  - **API 模式**:Anthropic API Key 自動解析(每份約 NT$ 1~3)
- 📋 自動填入生涯健診表 9 大類別共 60+ 欄位
- ✅ 可手動勾選保險雙十原則、家庭責任、豁免項目
- 🖼️ 匯出官方版健診表 PNG / 列印 / iOS 分享(LINE、Mail)
- 💾 結果僅存於本機 IndexedDB(離線、不上雲端)
- 📱 加到主畫面後就像原生 App

---

## Claude Pro vs API 差異

| | Claude Pro | Anthropic API |
|---|---|---|
| 計費 | 月費 | 用多少付多少 |
| 在這個 PWA | 用「手動模式」 | 用「API 模式」 |
| 自動化 | 需複製貼上 1 次 | 完全自動 |
| 適合 | 偶爾使用 | 大量使用 |

**Pro 訂閱無法呼叫 API。**這是兩個獨立產品。但「手動模式」讓你可以照常用 Pro。

---

## 部署(必須 HTTPS,iOS 才能裝 PWA)

### 最簡單:GitHub Pages(免費)

1. 開新 repo,把全部檔案丟進去
2. Settings → Pages → 從 `main` branch 部署
3. 拿到 `https://你的帳號.github.io/repo名稱/`
4. iPhone 用 Safari 開
5. 下方分享按鈕 → 「加入主畫面」

### 其他選擇

- **Cloudflare Pages**:`wrangler pages deploy .`
- **Vercel / Netlify**:拖檔即部署

---

## 使用流程(手動模式 / 推薦)

1. 第一次:「設定」→ 填顧問姓名 + 電話(會顯示在匯出的健診表上)
2. 「上傳」分頁 → 點「複製提示詞」
3. 開 claude.ai → 上傳 PDF + 貼提示詞 → 送出
4. 把 Claude 回的 JSON 整段複製
5. 回到 PWA「上傳」分頁 → 貼到下方文字框 → 「解析」
6. 自動跳到「健診表」分頁
7. 點右上「匯出」→ 勾選雙十/家庭責任/豁免 → 「下載 PNG」或「分享」
8. PNG 可直接傳 LINE 給保戶

## 使用流程(API 模式)

1. 「設定」→ 解析模式切到「API 模式」→ 填 Anthropic API Key
2. 「上傳」分頁 → 選 PDF → 「開始解析」
3. 自動完成

---

## 匯出格式

- **PNG**:1100px 寬 × 2x retina,適合 LINE 傳給保戶
- **列印 / PDF**:用瀏覽器列印功能存 PDF (A4 橫向)
- **分享**:iOS 原生分享選單,可直接送 LINE / Mail / AirDrop

---

## 客製化

- 改欄位:編輯 `index.html` 中的 `SCHEMA` 物件
- 改提示詞:編輯 `buildPrompt()` 函式
- 改顏色:編輯 `SCHEMA` 中各區塊的 `color` 欄位

---

## 隱私聲明

- API Key:localStorage(只存裝置)
- PDF 內容:
  - API 模式:傳到 Anthropic 解析完即釋放
  - 手動模式:你自己決定要不要上傳到 claude.ai
- 解析結果:IndexedDB(只存裝置)
- 沒有任何第三方追蹤、分析、雲端同步

---

## 已知限制

- 投資型保單、利率型壽險的「基本保額」與「實際保額」差異需人工複核
- 範圍值(如住院日額 2,000~3,000)目前取最高值
- 匯出 PNG 需要 html2canvas (從 CDN 載入,第一次需要網路)
- iOS Safari 須 15+ 才支援 Web Share API 分享圖片
