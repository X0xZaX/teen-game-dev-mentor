# Racing Lucas — 真實專案文件 / Real project documents

這四個檔案是用本 skill 開發 Racing Lucas（9 關第一人稱 3D 賽車遊戲）到 v0.8b 時的原始文件，未經整理，保留所有踩過的坑。
These four files are the actual, untouched documents from building Racing Lucas (a 9-stage first-person 3D racing game) to v0.8b with this skill, holes and all. Traditional Chinese.

- `SPEC.md` — 規格書 v8：84 條驗收標準、所有產品決定、★ AI 提案 / Spec v8: 84 acceptance criteria, every product decision, ★ AI proposals
- `CHANGELOG.md` — v0.1 → v0.8b 每版改了什麼、根因 / what changed in every version, with root causes
- `TESTPLAN.md` — 人工測試表＋fps 紀錄 / manual test table + fps log
- `HANDOFF.md` — 交接文件：約定、踩過的坑、下一步 / handoff: conventions, lessons, next steps

值得看的一段 / Worth reading: `HANDOFF.md` 第 4 節的符號錯誤——一個 `curv` 正負號反了，AI 用同一套假設驗證自己三次都「通過」，玩家體感說不對，繞了四個版本才抓到。這是 skill 裡「三層邊界」規則的來源。
Section 4 of `HANDOFF.md`: one inverted sign on `curv`; the AI "verified" its own fix three times with the same assumption while the player's feel said otherwise; four versions to find. This is where the skill's "three-tier boundary" rule comes from.
