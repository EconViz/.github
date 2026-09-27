# EconViz Ecosystem Roadmap

紀錄 EconViz 組織的套件版圖規劃與架構決策，供日後對照、避免重複討論同一個問題。

## 套件版圖

| 套件 | 範圍 | 狀態 |
|---|---|---|
| `econ-viz` → `utility-viz` | 效用函數、無異曲線、MRS、消費者均衡、Edgeworth box | 已發布（PyPI `econ-viz`），改名為 `utility-viz` 進行中 |
| `principle-viz`（原 `principle-econ`） | 大一經濟學：供需圖（連續/離散）、租稅與補貼（定額/從量/從價）、彈性、福利分析（CS/PS/DWL）、價格管制、數量管制 | 已改名+換 uv，數量管制與離散供需輸入待補 |
| `micro-viz` | 個體經濟學（不含效用函數本身）：Consumer Choice（budget/Marshallian·Hicksian demand/Engel/PCC·ICC/Slutsky decomposition）、Revealed Preference（WARP/SARP/GARP）、Producer Theory（isoquant/isocost/cost minimization/profit maximization）、Cost Theory（AC/AVC/MC）、General Equilibrium（Edgeworth box、contract curve、core，吃 `utility-viz` 的 preference object） | 規劃中 |
| `macro-viz` | 總體經濟學：Classical（labor market/production/loanable funds/quantity theory）、Keynesian（Keynesian cross/IS-LM/AD-AS）、New Keynesian（三方程模型）、Growth（Solow/Ramsey）、Dynamics（ODE/difference equation/phase diagram）、Quadrant（多象限聯動圖）。DGE/DSGE 不放在這裡 | 規劃中 |
| `trade-viz` | 國際貿易：autarky/world price/imports·exports/tariff/quota/trade welfare | 規劃中 |
| `dsge-viz` | 動態（隨機）一般均衡：deterministic 層（DGE：household/firm optimization、transition dynamics、saddle path）+ stochastic 層（DSGE：linearization、state-space、IRF、simulation）。DGE 是 `dsge-viz` 內的 deterministic 特例，不獨立成套件 | 規劃中 |
| `bezierkit` | 純數學 Bézier 曲線工具包（泛型 degree/dimension，不做 renderer）。`utility-viz` v2.x 的 tikz 輸出依賴它 | 進行中，見 `bezierkit-implementation-plan.md` |
| `econ-viz-theme` | 從 `utility-viz` 抽出的共用 Theme/Config 層（顏色/線寬/marker/label/legend 資料類別 + TOML 設定載入器），供多套件共用視覺系統 | 規劃中 |

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

明確排除國貿、外部性（externality）、廠商理論（perfect competition/monopoly）——這些各自有更適合的位置（`trade-viz`、之後可能的 `IO-viz`/`micro-viz`）。`principle-viz` 只做單一市場的基礎供需分析。

### 5. `micro-viz` 不重做效用函數

`micro-viz` 的 General Equilibrium 模組吃 `utility-viz` 的 preference object（例如 `CobbDouglas`、`CES`），不自己重寫一套偏好表示。依賴方向：

```text
utility-viz  →  preference / utility geometry
micro-viz    →  choice / demand / equilibrium（架在 utility-viz 的 preference 上）
```

### 6. DGE/DSGE 不獨立成三個套件，也不塞進 `macro-viz`

`macro-viz` 是理論圖（均衡、相圖、四五象限、ODE），`DSGE` 需要 log-linearization、state-space、Blanchard–Kahn condition 這類完全不同的技術機底，硬塞會讓 `macro-viz` 變形。最終只切兩個：`macro-viz`（教科書理論模型）+ `dsge-viz`（DGE 當它內部的 deterministic 特例）。

### 7. `bezierkit` 定位與依賴方向

純幾何/數學套件，不綁 `utility-viz`，不做 renderer（renderer 是 matplotlib/tikz/SVG adapter 各自的事）。`utility-viz` v2.x 的 tikz 輸出依賴它做曲線幾何計算，因此排在其他新套件之前先做。依賴方向：

```text
utility-viz (v2.x tikz 輸出)  →  bezierkit  →  numpy
```

## 決策日期

2026-09-27 ～ 2026-09-28，經與 Anthony Sung 討論確定。
