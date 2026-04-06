# Gene's Portfolio

單檔 HTML + 靜態資產的個人 Portfolio 網站，為部署到 Vercel 而準備。

---

## 🎯 網站的核心任務

**讓面試官和潛在合作者在 2–3 分鐘內決定是否聯繫 Gene。**

所有設計決策都服務這個目標。每次評估新功能或新內容時，先問：「這個改動會影響訪客『要不要聯繫他』的決策嗎？」如果不會，就不急著做。

---

## 📁 檔案清單

```
portfolio/
├── index.html              ← 網站主體（約 2000 行，HTML + CSS + JS 全包在一個檔案）
├── profile.jpg             ← Hero 區的個人照片（400×400）
├── favicon.png             ← 瀏覽器分頁 icon（64×64）
├── favicon-32.png          ← 瀏覽器分頁 icon（32×32）
├── og-image.png            ← 社群分享預覽圖（1200×630）
├── shaped-01-start.png     ← Shaped 專案截圖：開始頁
├── shaped-02-play.png      ← Shaped 專案截圖：遊玩中（三支滑桿）
├── shaped-03-round.png     ← Shaped 專案截圖：單輪結算（也是卡片封面 thumbnail）
├── shaped-04-final.png     ← Shaped 專案截圖：全局結算
└── README.md               ← 這份文件
```

9 個檔案全部放在同一層目錄（另加 `README.md`、`vercel.json`、`.gitignore`），不需要 build step。

---

## 🚀 部署到 Vercel

這是一個 zero-config static site，直接部署：

### 方法 1：Vercel CLI（最快）
```bash
npm i -g vercel
vercel
```
在專案根目錄執行 `vercel`，選 "deploy"，完成。

### 方法 2：GitHub + Vercel Dashboard
1. 把這個資料夾 push 到一個 GitHub repo
2. 在 [vercel.com](https://vercel.com) 點 "Add New Project"，選這個 repo
3. Framework Preset 選 "Other"，其他設定保持預設
4. Deploy

### 部署後必做：更新 og:image 為完整 URL
現在 `index.html` 裡的 og:image 是相對路徑 `og-image.png`。部署完拿到域名後，把 `<meta property="og:image">` 的 `content` 改成完整 URL，例如：

```html
<meta property="og:image" content="https://your-domain.vercel.app/og-image.png">
```

這樣 LINE / Facebook / Twitter 分享才會正確抓到預覽圖。

---

## 🚧 上線前必須補的資料（目前是 `#` 或佔位）

在 `index.html` 的 `SITE_DATA.contact.links` 裡找：

```js
contact: {
  links: [
    { icon: "📧", label: "Email", href: "mailto:basuya95@gmail.com" },  // ✅ 已填
    { icon: "💼", label: "LinkedIn", href: "#" },                        // ❌ 要補 LinkedIn URL
    { icon: "🐙", label: "GitHub", href: "#" },                          // ❌ 要補 GitHub URL
  ],
},
```

---

## 🏗️ 架構速覽

### 單檔哲學
整個網站是一個 `index.html`，把 HTML、CSS、JS 全部塞進去。沒有 build、沒有 npm、沒有依賴。改內容只要編輯這一個檔案。

### 內容與渲染分離
所有會想修改的文字、連結、列表都集中在檔案頂部的 `SITE_DATA` 物件裡。下方的 `render()` 函式負責把它變成 DOM。**改內容只要動 `SITE_DATA`，不用碰 render 邏輯。**

### 頁面結構
```
Status Bar（open to work / 時間 / caffeine / location）
Nav（Experience / Projects / After Hours / About / Contact）
Hero（照片 + slogan + CTAs）
Experience（4 段工作經驗，第一段預設展開）
Projects（2 個 side projects：豬魔覓食、Shaped）
☾ divider
After Hours（讀書筆記、原創劇本、短文集 28 篇）
About
  - How I Work（工作哲學，不是履歷背景）
  - Things I Care About（3 條 + footnote punchline）
  - Currently（4 條近況）
  - Education（數學系 → 經濟所，帶 note）
Contact（Let's talk + email/LinkedIn/GitHub）
Footer
```

### 設計系統
```
--bg: #0c0c0e             深色背景
--bg-raised: #141416      卡片背景
--accent: #c8a46e         金色（主 accent）
--ah-accent: #6bbfb3      teal（After Hours 專用）
```

字體：Playfair Display（顯示）/ Noto Sans TC（內文）/ DM Mono（UI/標籤）。

max-width：780px。全站單欄，沒有側欄。

---

## 🎮 遊戲化元素（共 11 個互動、8 個成就）

1. 頂部金色 scroll progress bar
2. Nav active state（滾到哪 nav 對應連結變金色）
3. 滑到底部 → 🏆 成就
4. 展開所有 Experience → 📜 成就
5. 點開任一 Project → 🔍 成就
6. 打開短文集 → ✍️ 成就
7. Hover footer 彩蛋 ▪▪▪ → 🥚 成就
8. 點 Caffeine 進度條（每次 +7%，到 100% overdose）→ ☕ 成就
9. 點 Footer 年份倒退 → ⏳ 成就
10. Hover 照片 → 「本人比照片上好看一點」tooltip
11. 解鎖前 7 個成就 → 🎖️ 隱藏成就

**原則：不要再加新的隱藏互動。** 11 個已經是 Rams 原則的上限。每多一個邊際價值趨近於零、但複雜度是線性增加。

---

## ❌ 已經決定不做的事（不要舊事重提）

- **Light mode toggle** — 深色主題是刻意選擇，是品牌的一部分
- **彈出視窗收集反饋** — 跟「讓人自己探索」的哲學衝突
- **成就清單頁面** — 破壞探索感、變成打勾 checklist
- **把 "I build" 改成 "Let's build"** — slogan 的工作是建立能力可信度，不是邀請
- **Banner 圖** — 會讓 Hero 變得像通用科技感素材
- **"After Hours" 改名** — 統一用 "After Hours"
- **再加新的隱藏互動** — 11 個已經是上限
- **把 About 做成展開/收合** — 展開交互為 Project 那種「可選的深入內容」保留。About 的解法是砍內容、不是藏內容。

---

## ✅ 最近做過的設計決策（影響後續改動的基礎）

### Project 卡片的互動模型
- 每張 Project 卡片分成 `.project-head`（name / type / desc / thumbnail / tags）和 `.project-detail`（Problem / Solution / Highlights / Gallery / Links）
- **點 `.project-head` 切換展開 / 收合**
- **點 `.project-detail` 不會觸發任何事**（讀者在讀內容時不會誤收合）
- 兩種收合路徑做不同的 scroll 處理：
  - 點 head 收合：保留 head 的視窗位置（0px jump）
  - 點「← 收合」按鈕：把 head 拉到視窗頂部 y=120 附近
- 這個模式和 Experience 的「點 header 切換」一致

### Project Schema 支援 `variant` 和 `gallery`
```js
{
  emoji: "🔷",
  name: "Shaped",
  type: "web game · 2026",
  desc: "...",
  thumbnail: "shaped-03-round.png",  // 卡片封面
  variant: "portrait",                // 直式截圖用 "portrait"（固定 180px 高 + cover 裁切）
  tags: [...],
  detail: {
    problem: "...",
    solution: "...",
    highlights: [...],
    gallery: [                        // 展開後 2x2 grid 顯示
      "shaped-01-start.png",
      "shaped-02-play.png",
      "shaped-03-round.png",
      "shaped-04-final.png",
    ],
  },
  links: [...],
},
```

- `thumbnail` 單張時用 `variant: "portrait"` 適合直式截圖
- `detail.gallery` 是陣列，展開後以 2 欄 grid 顯示（手機上自動單欄）
- **展開時卡片封面會隱藏**（`.project-card.expanded .project-thumb { display: none }`），避免和 gallery 重複
- 詳細 schema 註解在 `SITE_DATA.projects` 上方

### About → How I Work
- About 區塊的第一個 block label 從 `BACKGROUND` 改成 `HOW I WORK`
- 原本是 263 字的履歷式自介，已經砍到 3 段工作哲學（約 130 字），每段都是有立場的主張：
  1. 做產品跟做 side project 是同一件事
  2. 幽默應該是產品的合法設計維度
  3. 好的產品經理應該常常被團隊反駁
- Things I Care About 從 4 條減到 3 條（刪掉和 How I Work 重複的「敲敲打打」那條）
- 「台大經濟所 + 行為經濟學」從 Background 搬到 Education 作為 `note`
- Education 的 schema 支援 optional `note` 欄位：
  ```js
  { school: "...", period: "...", note: "..." }
  ```

### Education 的數學系 → 經濟所敘事
大學部是**台大數學系**（不是經濟系，這是之前記錯的），2013–2017。研究所是台大經濟所，2017–2019，研究行為經濟學。兩條都帶 note 描述跨領域敘事弧。

---

## 💬 關於 Gene 的溝通風格（給下一個協作者）

- 會用口語描述問題，但對細節很敏銳
- 經常問「你怎麼看」——這時候應該給有立場的回答，不要只列選項
- 說「讓我消化一下」時不要逼他決定，等他回來
- 問「你覺得如何」時，回答要**以網站的核心任務為標準**，不要被美學偏好牽著走

當他問「一樣回到你是設計者，加上這個網頁有需要完成的任務，你覺得怎麼改」時，這是在請你做**目的導向的篩選**——從一堆建議裡挑出真正影響目標的幾個。**大部分建議都應該被砍掉。**

---

## 📝 待辦事項

### 上線前必須
- [ ] 補 LinkedIn URL（`SITE_DATA.contact.links`）
- [ ] 補 GitHub URL（`SITE_DATA.contact.links`）
- [ ] 部署後把 og:image 改成完整 URL
- [ ] 決定部署平台（建議 Vercel）

### 等 Gene 準備好
- [ ] 更多 side project（目前 2 個）
- [ ] 如果想加新 project 的截圖，參考 Shaped 的 thumbnail + gallery 模式

### 可選的 polish
- [ ] 如果覺得 Projects 區只有 2 個太空，可以考慮 `emptySlots` 欄位（現在是 0）顯示「Coming soon」佔位卡
