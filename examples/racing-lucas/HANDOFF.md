# Racing Lucas — 專案交接文件（HANDOFF）
> 產生日期：2026-10-30 ｜ 目前版本：**v0.8b（定版，驗收通過）** ｜ 用途：讓新的對話／新的開發環境無縫接手

---

## 1. 這是什麼
- 單一 HTML 檔的第一人稱 3D 賽車遊戲（three.js r159 內嵌，離線可玩），另有 Netlify 靜態部署包。
- 工作流程：`/teen-game-dev-mentor` skill——Isaac 是產品負責人，Claude 是教練＋工程團隊。每版流程：**SPEC 對齊 → 開發 → 交付報告（✅/⚠️/❌/❓、「沒驗證的」、「怎麼測」）→ Isaac 實機驗收**。每則回覆結尾有進度框（📍進度 / 📦版本 / 📄你手上應該有 / ⏭️下一步）。
- 文件三件套在 `deliver/`：`SPEC.md`（v8，含所有產品決定與驗收條目 #1–#84）、`CHANGELOG.md`、`TESTPLAN.md`（人工測試表＋fps 紀錄）。

## 2. 目前內容（v0.8b）
- 9 關、3 主題：1–3 陽光草地（山地／湖畔／峽谷）、4–6 城市（河岸高架白天／夜市夜晚／環城快速道路 120 秒晝夜循環，東京街頭＋富士山）、7–9 Cyber Punk（霓虹環 NEON LOOP／螺旋塔 CORKSCREW／核心 THE CORE）。
- 第 7–9 關使用**貼軌引擎**：垂直 Loop、斜坡彎（向內傾 ≤ 14°、R ≥ 127）、Helix、720° Corkscrew、倒掛天花板 600 m、斷橋跳躍（速度不足→掉落→重生在斷口前 120 m）、Boost pad、第 9 關第 3 圈「核心過載」＋異宇宙縫隙（即時 portal 渲染）、破關動畫（環繞鏡頭、煙火、字幕）。
- 系統：生涯 9 關前 3 名晉級、積分、車庫 10 台車（性能實際生效）、難度模式（爽快／挑戰）、雙人分割畫面（9 條賽道）、Booster、手煞車、小地圖、畫質三段（G）、動態模糊開關（B）、翻轉視角開關（V）、Eurobeat＋Darksynth 原創合成音樂、存檔（localStorage）。
- 後製管線：HDR → SSAO → 光暈 → ACES 色調 → 深度感知動態模糊 → 鏡頭光斑 → FXAA。

## 3. 程式結構（`rl/`）
```
src/sim.js      物理與賽道（無 three 依賴）：2D 賽道 buildTrack、貼軌 buildRailTrack、車輛物理、AI、碰撞、夜市道具、跳躍 stepAir、佈局 LAYOUTS
src/game.js     流程、HUD、攝影機、生涯／車庫／結算、破關動畫、測試鉤子 window.__RL
src/env.js      草地主題環境 + 共用 kit（makeCanvas/toTex/mergeGeos…）
src/city.js     城市主題（天空／日夜／路／橋／隧道／夜市）
src/tokyo.js    東京街區、富士山、夜市物理道具的渲染
src/cyber.js    Cyber Punk 主題（貼軌路面 ribbon、霓虹城、全像牌、斷橋、塔、核心、過載、portal 縫隙、車尾光軌）
src/post.js     後製管線
src/cars.js     10 台車模型與塗裝；src/cockpit.js 駕駛艙與儀表；src/audio.js 合成音效與音樂
src/template.html  頁面骨架與 CSS（HUD、選單、動畫）
build.py        把 three.min.js + src/*.js 串進 template → dist/RacingLucas_<ver>.html（單檔）
build_web.py    把 dist 包成 web/racing-lucas-<ver>/ 與 deliver/RacingLucas_<ver>_netlify.zip（VER 常數要改）
test/           simtest.js、tracktest.js、railtest.js（node）；browser_test / twop_test / career_test / garage_test / v06 / v07a / v07b / v08a / v08b（Playwright + swiftshader）
```
修改方式：直接改 `src/`，不是改 dist；版本號在 `game.js` 的 `VERSION`、`build.py` 檔名、`build_web.py` 的 `VER` 三處。

## 4. 關鍵技術約定（新接手的人一定要知道）
- **座標／符號**：three.js 相機看 +z 時 +x 在畫面左邊；2D 賽道 `curv > 0` = 航向往 +x 轉。貼軌引擎的 `curv` **必須**跟這個一致（v0.8a 曾反號，造成斜坡外傾＋車自己多轉進彎，繞了 4 版才抓到）。驗證不要用引擎自己的符號，用「實際航向變化方向」與「追焦截圖」——已寫進 `test/railtest.js`（curvSign / banking 檢查）。
- **貼軌引擎**：車在「攤平座標」模擬（x = −lat、z = s、h = 相對軌道航向），世界姿態由 frame（T/U/Nv）算出存在 `c.W`。路面轉彎 = 車相對軌道反向偏航（`rot = −curv·v·dt`）。重力：切線分量影響速度；橫向分量（`P.railLatG = 1`）在斜坡上生效；法線方向磁吸（規則 A，不會掉）。斷橋是唯一離開路面的地方。
- **真實物理原則（產品決定）**：轉彎一定要打方向盤；斜坡一律向內傾、只用在大半徑、全速可過；小半徑彎平地＋煞車提示；沒有「下壓模式」之類的街機自動跟彎。
- **第 1–6 關物理零改動**：`railtest.js` 會從 `dist/RacingLucas_v0.7b.html` 抽出舊模擬程式，6 條賽道逐位元比對；`simtest.js` 輸出應與 `shots/v07b/reg/sim.json` 完全相同。
- 路面可辨識性：Cyber 路面自發光＋每 8 m 橫向光紋（Isaac 要求「人眼要看得到賽道」）。
- 道具／光軌／縫隙都是 InstancedMesh 或合併幾何；draw call 目前 600–800（第 7–9 關）、三角形 20–27 萬；第 5 關夜市 110–160 萬（最重）。
- 測試機是軟體渲染（swiftshader），**fps 無法量**；M4 實測 v0.7a 高畫質約 30 fps。

## 5. 產品決定紀錄（摘要，細節在 SPEC.md）
- 車造型保留、貼圖升級；接受較低 fps 換畫質；檔案 ≤ 10 MB 可接受。
- 夜市道具各自讀秒重生（落地 20 s 淡出、5 s 後原位重生）。
- 貼軌：規則 A 磁吸；斷橋跳躍要；第 9 關第 3 圈過載要；縫隙要看到「另一個宇宙」；v0.8 分 a／b 兩段交付。
- Boost pad 實作為「免費 1 秒 Booster 不扣次數」（原 SPEC 寫 +25% 能量）。

## 6. 已知缺口／backlog
- M4 實測 fps（第 7–9 關、第 9 關過載、雙人）尚未紀錄。
- draw call 偏多：全像牌、光環、無人機各自一個 mesh；可合併。
- 無手把支援（鍵盤 0/1 方向盤是高速彎「飄」感的最後來源）。
- 沒有「人類手感」自動測試（AI 是閉迴路，什麼 bug 都能跑過）。
- 存檔無匯出／匯入；沒有幽靈車、分段計時、季賽。
- backlog：科技樹改裝（Isaac 提議，見下）、零件升級、後照鏡、手把、雨天、鏡像／逆向賽道、線上排行榜（需後端，先不做）。

## 7. 討論中的下一階段（2026-10-30 的結論，尚未定案）
1. **先切換開發模式**：原始碼放到 Isaac 的 Mac（git repo）＋ Claude Code / VS Code。理由：目前程式碼只在雲端暫存區（已給 `RacingLucas_source_v0.8b.zip` 備份）；真 GPU 上跑測試與 fps；省 token、不再依賴對話壓縮後的摘要。
2. **v0.9 地基**：拆成多檔靜態站（仍 Netlify、仍離線）、效能到高畫質 60（合併 draw call、遠景 LOD、後製半解析度、過載貼圖降頻）、手把支援、人類手感測試、存檔匯出匯入。
3. **v1.0 科技樹改裝**（Isaac 的提案）：架空科技五條線（動力／推進／空力／底盤／護盾，每線 4–5 階，零件看得見）、積分＋成就雙鑰匙解鎖、每線最後一階互斥形成流派、挑戰模式 AI 隨科技等級升級、10 台車變成「底盤」。需要先有真機上的自動平衡工具（各流派 9 關圈速表）。
4. 新主題關卡放 v1.1。

## 8. 檔案清單（Isaac 手上應有）
- `RacingLucas_v0.8b.html`、`RacingLucas_v0.8b_netlify.zip`
- `SPEC.md`（v8）、`CHANGELOG.md`、`TESTPLAN.md`、本檔 `HANDOFF.md`
- `RacingLucas_source_v0.8b.zip`（src、test、build 腳本、three.min.js、文件）
