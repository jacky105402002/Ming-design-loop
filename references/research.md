# 研究依據與採用決策

查核日期：2026-10-15。這些是研究來源，不是自動執行或安裝清單。Ming-design loop 使用獨立撰寫的規則，未打包第三方技能原文、安裝器或執行檔。未替第三方專案提供安全或授權認證；若日後要安裝或複製其資產，重新核對來源、版本、授權及本機安全流程。

## 主要來源

| 來源 | 本次採用 | 本次調整／不採用 |
|---|---|---|
| [Anthropic frontend-design](https://github.com/anthropics/skills/blob/dbd4588f9e1033efb41dad4bef2f7947c8993d44/skills/frontend-design/SKILL.md) | 從實際產品內容形成方向、先規劃再檢查、注意文字與視覺層次 | 不把每個案子都假設成追求強烈風格的設計委託；不自動產生正式前端程式 |
| [Impeccable](https://github.com/pbakaus/impeccable/blob/ff8e1495979a33086089c9c9f7cfea2ddc386e97/.agents/skills/impeccable/SKILL.md) | 區分畫面使用目的、局部細修保留原設計、集中檢查與修正 | 不引入 launcher、binary、hooks 或自動下載；不以「允許設計」擴張操作授權；必要檢查未過不能因輪數上限而結案 |
| [Better Design](https://github.com/marvkr/better-design) | 給 AI 明確的設計變數、元件與檢查依據 | 不把品牌主題當作整套 UX；不綁定遠端 MCP 或 shadcn |
| [Design Taste](https://github.com/madebymustafa/design-taste) | 風格要有可解釋的方向，避免無目的套模板 | 不把任何風格預設成所有產品標準，也不一次疊加多套相衝突的設計指令 |
| [Superdesign Skill](https://github.com/superdesigndev/superdesign-skill) | 探索草稿、保留設計脈絡與後續迭代 | 不依賴其帳號、雲端產圖或 CLI；Figma 交付直接由可用的 Figma 工具處理 |
| [Figma Console MCP](https://github.com/southleft/figma-console-mcp/blob/main/docs/setup.md) | 可編輯設計、元件／變數操作、讀回與截圖驗證 | 工具能力與連線每次核實；不把工具存在當成功寫入證據 |

前兩項閱讀了固定提交的 SKILL.md；其餘以官方 README／工具文件確認定位，沒有完整審閱全部程式與相依套件。上游分支連結可能改變，採用新功能前重新查核。

## Threads 的其他資源

[Canvas UI](https://github.com/DavidHDev/canvas-ui)、[Paper Shaders](https://github.com/paper-design/shaders)、[UI Layouts](https://github.com/ui-layouts/uilayouts)、[Hyperiux](https://github.com/Hyperiux-Immersion-Labs/hyperiux-components)、[Motion Primitives](https://github.com/ibelick/motion-primitives)、[Cursify](https://github.com/ui-layouts/cursify)、[Trickle UI Kit](https://github.com/akaieuan/trickle-UI-kit)主要提供元件或效果，按具體畫面需要作參考。它們不構成本流程必裝依賴；採用時把效果目的、可行性、授權與替代呈現交給開發判斷。

## 本機習慣的轉換

- 繁體中文溝通，先說結果、影響與下一步，技術名詞首次用白話解釋。
- 先查來源與現有規範；低影響選擇自行處理，重要產品缺口才詢問。
- 已授權範圍自主推進；階段通過不例行要求人工同意。
- 不覆寫外部設計師原稿，不把作品來源等同操作授權。
- 低預算調整工作量，不降低必要驗收；與設計師合作可跳過已完成的設計階段。
- 真正做過的檢查才報通過，自我審查如實標示；失敗有依據地修正，保留未完成項。
- 專案事實、版本與進度留在產品專案，不寫進全域 Skill。
- 與 MingMing 共用需求來源與主線狀態，設計透過規格及證據銜接，不改主線 schema。

## 參考技能的靜態檢查

本次以本機 scan-skill-security 1.0.2 檢查兩份尚未安裝的來源文字與 provenance，各兩個檔案，沒有執行其命令或安裝其依賴。

- Anthropic：規則掃描未發現風險指標，coverage 2／2；這不是安全保證。
- Impeccable：原始掃描 verdict 為 BLOCK，`SECRET_COLLECTION` 命中 SKILL.md 第 3 行。人工讀取上下文後，該行是設計技能描述，提到 extract 與 design tokens 等設計功能，未觀察到索取憑證或指定外洩目的地的指令；記為疑似詞彙誤判，保留原始報告，不竄改為通過。其入口另有首次執行下載 binary 的行為描述，本次未執行、未採用。

掃描範圍只有選定文字，不包括引用的子文件、binary、hooks、相依套件或整個 Git 歷史。此結論只支持靜態研究取捨，不授權安裝陌生技能。來源 commit 與雜湊收錄在本次交付的驗證資料。
