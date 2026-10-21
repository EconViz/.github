# EconViz Ecosystem Roadmap

紀錄 EconViz 組織的套件版圖規劃與架構決策，供日後對照、避免重複討論同一個問題。

## 套件版圖

| 套件 | 範圍 | 狀態 |
|---|---|---|
| `econ-viz` → `utility-viz` | 效用函數、無異曲線、MRS、消費者均衡、Edgeworth box | 已發布（PyPI `econ-viz`），改名為 `utility-viz` 進行中 |
| `principle-viz`（原 `principle-econ`） | 大一經濟學：供需圖（連續/離散）、租稅與補貼（定額/從量/從價）、彈性、福利分析（CS/PS/DWL）、價格管制、數量管制 | 已改名+換 uv，數量管制與離散供需輸入待補 |
| `micro-viz` | 個體經濟學（不含消費者理論與一般均衡，兩者在 `utility-viz`）：Producer Theory（isoquant/isocost/cost minimization/profit maximization）、Cost Theory（AC/AVC/MC）、Market Structure（完全競爭/獨占/價格歧視/Cournot·Bertrand·Stackelberg）、Uncertainty（期望效用/風險貼水/state-contingent claims/隨機優勢）、Information（逆選擇/道德風險/mechanism design/拍賣）。不含賽局理論 | repo 已建立（private），roadmap 見 micro-viz#2 |
| `macro-viz` | 總體經濟學（給定方程式、畫出來）：國民所得與景氣資料、Keynesian cross、AD-AS、IS-LM、貨幣市場、Phillips curve、Solow 成長、開放經濟、四五象限聯動圖、Dynamics（2D ODE phase portrait、差分方程、蛛網圖）、靜態 New Keynesian 三方程。DGE/DSGE 不放在這裡 | repo 已建立（private），roadmap 見 macro-viz#2 |
| `trade-viz` | 入門版以上的國際貿易（小國局部均衡的 autarky/world price/tariff/quota 已在 `principle-viz`）：古典（Ricardo、多商品鏈、offer curve）、新古典（Heckscher–Ohlin、Stolper–Samuelson、Rybczynski、FPE、specific factors）、貿易政策、新貿易理論（Krugman）、新經濟地理、Melitz、gravity | repo 已建立（private），roadmap 見 trade-viz#2 |
| `game-viz` | 賽局理論：輸入賽局後匯出報酬矩陣與賽局樹（LaTeX，模板由維護者提供）；之後依序擴充求解與標記（Nash、逆向歸納）、不完全資訊、重複賽局、合作賽局與議價、拍賣與機制設計、社會選擇、演化賽局。經濟情境的寡占反應曲線留在 `micro-viz`，策略性貿易政策留在 `trade-viz` | repo 已建立（private），roadmap 見 game-viz#2 |
| `dsge-viz` | 需要推導與求解的動態模型：deterministic 層（DGE：household/firm optimization、Ramsey、saddle path、transition dynamics）+ stochastic 層（DSGE：linearization、Blanchard–Kahn、IRF、simulation）。DGE 是 `dsge-viz` 內的 deterministic 特例，不獨立成套件、不獨立開 repo。可單向呼叫 `macro-viz` 畫 phase portrait | repo 已建立（private，空），未動工 |
| `bezierkit` | 純數學 Bézier 曲線工具包（泛型 degree/dimension，不做 renderer）。`utility-viz` v2.x 的 tikz 輸出依賴它 | repo 已建立（private，空），已 clone 到本機，實作計畫見 `bezierkit-implementation-plan.md`（本機 `~/Downloads`），尚未動工 |
| `econ-viz-theme` | 從 `utility-viz` 抽出的共用 Theme/Config 層（顏色/線寬/marker/label/legend 資料類別 + TOML 設定載入器），供多套件共用視覺系統 | repo 已建立（private，空），未動工 |

## 架構決策

### 1. 為什麼拆這麼多套件而不是一個大的 `econ-viz`

`econ-viz` 一開始只做消費者理論（效用/無異曲線），範圍夠窄、抽象一致。往下要做的供需圖、總體理論、國貿、DSGE 是完全不同的圖表文法，硬塞進同一套 Canvas 抽象只會讓 API 變形。依課程/主題拆套件，也讓使用者可以只裝自己要的部分。

### 2. `econ-viz` 改名 `utility-viz`

見 `rename-utility-viz` 決策：套件族一多，`econ-viz` 這個名字太籠統，`utility-viz` 精準對應它實際在做的事（效用/無異曲線消費者理論），把其他經濟學領域的名字空間留給其他套件。

### 3. 共用層何時抽、抽什麼

**原則**：不要在只有一個真實使用者時預先抽象。等第二個真實用例出現、看得到具體重複時才抽，避免抽錯邊界導致所有下游套件跟著重切一次。

**目前的判斷**：
- `econ-viz-theme`（Theme + Config）現在就該抽——`principle-viz` 已經獨立發明了一套幾乎同構的 `plot/theme.py`/`plot/colors.py`（含 monochrome palette），這是具體重複，不是假設。
- Canvas（畫圖引擎本體）先不抽——`utility-viz` 的 Canvas（無異曲線/預算線/均衡點幾何）跟 `principle-viz` 的 `MarketFigure`（供需圖幾何）是兩套獨立設計，現在統一等於重寫 `principle-viz` 整個畫圖層，成本高、時機未到。等 `micro-viz`/`macro-viz` 生出來，Canvas 層的共通邊界才會清楚。

### 4. `principle-viz` 範圍收斂

`principle-viz` 做單一市場的基礎供需分析，並涵蓋入門層級的小國貿易（free trade、tariff、quota）、外部性、公共財、PPF 與可貸資金。廠商理論（perfect competition/monopoly）在 `micro-viz`；中高級國貿（Ricardo、Heckscher–Ohlin、新貿易理論等）在 `trade-viz`，不與 `principle-viz` 的小國局部均衡圖重複。

### 5. `micro-viz` 不重做消費者理論

消費者選擇（budget、demand、Slutsky、revealed preference）與一般均衡（Edgeworth box、contract curve）都在 `utility-viz`。`micro-viz` 只做廠商、市場、不確定性與資訊，需要偏好時吃 `utility-viz` 的 preference object（例如 `CobbDouglas`、`CES`），不自己重寫一套偏好表示。賽局理論不在 `micro-viz` 範圍內。依賴方向：

```text
utility-viz  →  preference / consumer choice / general equilibrium
micro-viz    →  firms / markets / uncertainty / information（架在 utility-viz 的 preference 上）
```

### 6. DGE/DSGE 不獨立成三個套件，也不塞進 `macro-viz`

切分規則：**方程式是給定的、只要畫出來 → `macro-viz`；模型要從 FOC 與約束式推導並求解 → `dsge-viz`。**

`macro-viz` 是理論圖（均衡、相圖、四五象限、ODE、差分方程），不需要求解器。`DSGE` 需要 log-linearization、Blanchard–Kahn condition、shooting 這類完全不同的技術底子，硬塞會讓 `macro-viz` 變形。因此 Solow 與通用 phase portrait 在 `macro-viz`，Ramsey 與 saddle path 在 `dsge-viz`。最終只切兩個：`macro-viz`（教科書理論模型）+ `dsge-viz`（DGE 當它內部的 deterministic 特例）。依賴方向單向：`dsge-viz` 可呼叫 `macro-viz` 的 phase portrait 繪圖，反之不行。

### 7. `bezierkit` 定位與依賴方向

純幾何/數學套件，不綁 `utility-viz`，不做 renderer（renderer 是 matplotlib/tikz/SVG adapter 各自的事）。`utility-viz` v2.x 的 tikz 輸出依賴它做曲線幾何計算，因此排在其他新套件之前先做。依賴方向：

```text
utility-viz (v2.x tikz 輸出)  →  bezierkit  →  numpy
```

## 決策日期

2026-09-27 ～ 2026-09-28，經與 Anthony Sung 討論確定。
