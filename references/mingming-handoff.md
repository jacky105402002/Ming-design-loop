# MingMing 與外部設計交接

## 責任界線

| 責任 | 主要來源／負責方 |
|---|---|
| 功能範圍、業務規則、AC、權限、資料與技術限制 | MingMing／產品需求負責方 |
| 使用者操作流程、線稿、視覺、互動與元件設計 | Ming-design loop／合作設計師 |
| 需求到設計到開發的對照與缺口 | MingMing uiux-designer 與本流程交接 |
| API、資料模型、正式程式及實際產品測試 | MingMing／既有開發流程 |

設計改變付款、資格、權限、保存、流程前置條件等語意時，提出需求差異，不自行改寫原始規則。架構先提供限制，再隨有依據的設計發現更新；設計與工程不各自定義同一個事實。

## 接入現有 MingMing

先讀本產品的 AGENTS、入口、版本、state、feature spec、handoff、uiux-designer、Definition of Ready 與證據規則。未導入時不因本 Skill 存在就自動導入；使用者明確選 MingMing 後才依其入口操作。

已查核的參考版本：MingMing **v1.1.3**，來源 commit `e91042baf58f1bf8325ebd562bc5e5e2fed36724`（2026-10-15）。依其 [版本說明](https://github.com/jacky105402002/MingMing-AI-Product-Engineering-Loop/blob/e91042baf58f1bf8325ebd562bc5e5e2fed36724/docs/releases/v1.1.3.md)，承接 v1.1.2-r1 的功能與資料契約，沒有新增 schema 遷移。本次確認文件層接口，尚未完成真實產品端到端驗證；既有專案沿用自己的版本，不自動升級。

搭配使用時由 MingMing 主線協調：在適用的 `uiux-designer` 階段 06 接入本流程，完成後回到主線的工作拆分、開發條件判定與執行。主線唯一更新 `tasks/current/state.json`；本流程只維護設計細節及交接，不建立另一個主線排程。獨立使用時不要求 MingMing 檔案、Node 或證據工具。

本流程以文件交接，沒有新增或假設 MingMing state 的 JSON 欄位，也不改其驗證器：

1. planner 提供已確認需求、AC、範圍與適用限制，達 Design Ready。
2. uiux-designer 選擇本流程或外部設計來源；指向本設計任務資料夾。
3. 本流程交付 design-handoff 與本次有效證據，列出映射與缺口。
4. 主線依自己版本的格式，把設計文件與基準快照列入相關 spec／evidence_files；引用實際路徑，不把本流程狀態當成主線新 schema。
5. 主線核對所有適用架構、API、資料等條件後才可標 Implementation Ready。**Design Ready／設計 ready 不等於 Implementation Ready。**
6. 開發後由既有流程執行操作、響應式、可及性與畫面比對；偏差有證據再修正。

需要修改 MingMing 共用模板、驗證器或自動路由時，另列明確修改範圍；完成本工作流不隱含此類改動。

## 設計證據如何接回主線

依產品版本的 [證據政策](https://github.com/jacky105402002/MingMing-AI-Product-Engineering-Loop/blob/e91042baf58f1bf8325ebd562bc5e5e2fed36724/ai-workflow/evidence-policy.md)與導入文件操作。下面是 v1.1.3 的對照範例，示例 ID 與路徑不是已存在的產品紀錄：

| 設計產物 | 主線接收方式 |
|---|---|
| `tasks/design/design-001/design-handoff.md` | 在相關規格與 Node 的 `evidence_files` 引用，讓需求／互動變更能被追蹤 |
| `tasks/design/design-001/evidence/r001/round-01/review.md` | 保留設計檢查原始報告；主線審查依本次 Node 範圍引用，不把它直接當 evidence JSON |
| Figma 節點、版本、時間與本地基準快照 | 在交接中記錄雲端來源；把影響實作的本地快照納入相關 `evidence_files`。只有網址無法讓檔案雜湊偵測雲端改稿，接續時仍需核對 |
| 主線 Node 的測試與審查 | 分別保存在 `tasks/current/reports/{node-id}/{attempt-id}/`，由主線建立證據與更新 check |

所有主線路徑以產品根目錄為基準。設計開始時可能尚未拆分 Node；先用 AC／FLOW／SCR 對照，待主線拆分後補上 Node 關聯，不為填表虛構編號。

主線完成當次實際審查、寫好 review.md 並判定通過後，才可依以下形式記錄；需替換成實際 Node、attempt 與檔案，失敗使用實際結果：

```sh
node scripts/record-evidence.mjs --node node-001 --kind review --out tasks/current/reports/node-001/attempt-01/review.json --artifact tasks/current/reports/node-001/attempt-01/review.md --result pass --reviewer-mode self-review
```

這個命令記錄已完成的審查，不會代替審查或自動把 Node 標為 done。測試另由主線執行真實專案命令並記錄；不能把設計走查包裝成瀏覽器、權限或可及性測試。相關 Node 的 spec、`evidence_files` 與依賴須涵蓋實際程式、契約及設計基準，不能只列報告。之後核對 evidence 的 Node、revision、結果與 AC 範圍，更新 check 並執行主線驗證。

**兩種不適用須分開：** 外稿完整可將重新製作線稿／UI 的設計階段記 `not_applicable` 並附理由；這不表示省略接收檢查，也不能讓 MingMing Node 的 `test`／`review` 變成不適用。文件任務可做適用的連結、格式與內容驗證；docs 是否不適用依主線規則判斷。

## 適用資安要求與元件對照

已知政府案或主線已有資安計畫時，引用專案確認的計畫版本、要求 ID 與來源，對照受影響畫面／狀態，以及交由開發驗證的事項。例如已有明確規則的資料遮罩、無權限提示、登入逾時與恢復流程；不因範例存在就把它變成所有專案必備功能。

設計只記錄畫面規格與實際設計證據，不自行把主線資安項目改為符合，也不代替執行紀錄與逐項內容覆核。要求未知時標明受阻範圍，交回主線／security-planner 確認；既有計畫或等級不自動遷移。無相關要求的獨立設計不強制建立資安計畫。

S0／S2 先讀產品既有元件與設計系統。React 專案按需讀取主線的 `ai-workflow/frontend-resource-map.md`；清單是候選，不表示套件已安裝或需要更換框架。交接記錄使用區域、實際元件、選擇理由、狀態、來源及動效的減少動態替代。技術相容性尚未查核就標未確認，不把候選當已完成的程式對應；非 React 專案沿用自己的元件來源。

## 接收外部設計

先核對設計擁有者、用途、指定版本與讀寫範圍。從 Figma、設計文件、原型、會議決議與附件收集足夠資訊，AI 自行映射到交接表。外部設計師不必另填整套本流程模板。

可直接交接的條件：需求與設計對應、適用畫面／狀態／裝置足夠完整、重要規則清楚、來源可取得。缺少主元件或 Auto Layout 是否阻塞，要看本次是否要求可編輯設計與後續維護；不得對只要求唯讀接收的外稿強制重構。

外稿缺少無權限頁面時，先確認是否已有權限規則，再提出缺少的設計。已有規則可整理狀態規格；不能自行發明角色權限。只授權接收與審查時，不擅自改稿。

## 最小交接契約

用 assets/design-handoff.md 或專案等價格式，至少覆蓋：

- 需求來源、版本、AC 與畫面／流程 ID 對照。
- 設計來源、作者、採用範圍、Figma 檔案與具體節點、基準時間／版本／快照。
- 互動：前置條件、觸發、結果、錯誤／取消／恢復，以及資料與權限的來源。
- 必要狀態、多裝置規則、內容極值與可及性意圖。
- 元件、樣式變數、素材與工程映射；未知程式元件路徑填未確認，不捏造 Code Connect。
- 實際檢查證據、未決項目、ready 的確切範圍，以及留到開發執行的檢查。

線稿交付不能宣稱視覺定稿；文字規格不能冒充 Figma 畫框。只有部分畫面 ready 時列出可用畫面與受阻畫面，分析共用元件／流程依賴後才允許分批開發。

## 修訂與證據失效

需求或設計 revision 改變時，列出受影響 AC、畫面、元件、依賴與已開發部分。舊稿保留；新稿與修正證據另存。

本流程將對應檢查標 stale，向主線交付變更影響。主線依自己的規則重開受影響 Node、建立新 revision／attempt 並重新驗證。不得沿用舊截圖、舊雜湊或客戶對舊稿的確認，冒充新稿證據。

目前版本是文件層接口；尚無跨工作流自動同步或 Figma webhook。接續時讀取引用檔案並核對差異，不能承諾背景同步。
