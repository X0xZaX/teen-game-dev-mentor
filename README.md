# Teen Game Dev Mentor

**繁體中文** | [English](README.en.md)

一個給青少年（和任何第一次做軟體的人）的 AI 導師 skill：**不寫一行程式**，只靠對話，把一個單網頁小遊戲從想法做到公開上線——同時學會「軟體產品負責人」的思考方式，以及「怎麼跟 AI 協作」。

> 這不是教寫程式的 skill。它教的是：先想清楚再動手、用量得出來的方式描述需求、用證據判斷完成、改動留紀錄、上線前測試、改動前知道代價——還有，怎麼讓 AI 寫規格然後看出哪裡不是你的意思、怎麼挑戰 AI 的證據而不是對錯、怎麼跟 AI 腦力激盪做出自己想不到的東西。

## 它是怎麼來的

這套方法來自兩個真實專案，由一個不寫程式的人只靠對話完成：

1. 一個 WWII 玩具兵的 3D 波次防禦射擊遊戲，做到公開試玩。
2. **Racing Lucas**：一個 9 關的第一人稱 3D 賽車遊戲（three.js、單一 HTML 檔、離線可玩），二十多個版本、84 條驗收標準、雙人分割畫面、貼軌引擎（垂直 Loop、720° 螺旋、倒掛天花板、斷橋跳躍）、破關動畫。

第二個專案把流程推到極限，skill 裡一半的規則是從那裡踩過的坑學來的——例如「AI 不可以用同一套假設驗證自己的修正」這條，來自一個方向符號錯誤繞了四個版本才抓到。`examples/racing-lucas/` 放的就是這個專案的真實文件（SPEC、CHANGELOG、TESTPLAN、HANDOFF）。

## 核心設計

| 機制 | 解決什麼問題 |
|---|---|
| **六站節奏**（開場 → 規格 → 開發 → 驗證迭代 → 公開部署 → 收尾交接） | 每站多一個檔案、學會一個概念；小而完整，不是大而精美 |
| **知識卡兩層制** | 名詞在「需要做決定的那一刻」才解釋（決策卡），其他的列在回覆底部，想懂再點；全部累積進 `GLOSSARY.md` |
| **挑戰證據的三層邊界** | 第一次沒修好先練回報品質；第二次 AI 必須承認驗證可能有盲點、只診斷不修；第三次叫停重看假設 |
| **動手前先報價** | 收到 bug 或需求，AI 先說改哪裡、規模、幾輪，等你點頭——省 token，也讓孩子知道每個決定的代價 |
| **★ AI 提案槽** | 每版 SPEC 保留一格讓 AI 提你沒想到的點子，附參照與成本；讓 1+1>2 變成流程而不是運氣 |
| **規模守門員** | 單網頁裝不下時（>1 MB、需要伺服器、系統級需求）AI 不接單，帶你定義 scale、切 MVP、或畢業 |
| **畢業門檻＝五個可觀察行為** | 不是「會寫規格」，是會審規格、會否決、會分類回饋、會問證據、會下指令 |
| **HANDOFF.md 每次對話必留** | 對話會消失、會被壓縮、會換模型；檔案就是存檔 |

## 安裝

### Claude（claude.ai / Claude Code）
- **claude.ai**：下載 `skills/teen-game-dev-mentor/`（或英文版 `skills/teen-game-dev-mentor-en/`），在設定的 Skills 頁面上傳資料夾。Team / Enterprise 管理員可以發給整個組織。
- **Claude Code**：把資料夾複製到 `~/.claude/skills/`，或用支援 Agent Skills 標準的安裝器：
  ```bash
  npx skills add X0xZaX/teen-game-dev-mentor
  ```

### 其他模型（ChatGPT、Gemini、本地模型…）
`SKILL.md` 就是純 Markdown。拿掉最上面 `---` 之間的 frontmatter，剩下的內文直接當系統提示（system prompt）或自訂指令貼上即可。內文不依賴任何 Claude 專屬工具，只假設模型能產出單一 HTML 檔與 Markdown 文件。若那個模型沒有「回覆結尾進度框」的習慣，第一輪提醒它一次。

## 怎麼開始

對 AI 說：

> 我想做一個遊戲。

就這樣。skill 會自我介紹、發第一張知識卡、問你三個問題，然後開始第 0 站。

帶著舊專案回來時，上傳 `SPEC.md`、`CHANGELOG.md`、`HANDOFF.md`、`GLOSSARY.md` 和最新的 HTML，用開發指令格式說你要做什麼：

> 依據 SPEC v3，做出 v0.4，要滿足驗收標準第 12 到 15 條，交付一個 HTML 檔。

## 資料夾

```
skills/
  teen-game-dev-mentor/SKILL.md      ← 繁體中文版
  teen-game-dev-mentor-en/SKILL.md   ← English version
templates/                           ← SPEC / TESTPLAN / CHANGELOG / HANDOFF / 交付報告 空白範本（雙語）
examples/racing-lucas/               ← 真實專案文件：SPEC v8、CHANGELOG、TESTPLAN、HANDOFF
```

## 給家長與老師

- skill 預設學習者是未成年人：上線前會確認不放真名、學校、照片；需要帳號（Netlify）時會提醒由家長建立。
- 它不會替孩子做決定。每個分岔給 2–3 個選項和代價，由孩子選。
- 它有成本意識：每次對話開頭問預算大小，動手前先報價。想省的時候說「今天省一點」。

## 授權

MIT。歡迎 fork、翻譯、改成你自己的版本；如果你用它帶了一個孩子做出遊戲，很想聽到。
