
<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Noto+Sans+TC&weight=900&size=40&pause=1000&color=7C3AED&center=true&vCenter=true&width=600&lines=%E2%9C%A8+%E7%B4%AB%E5%BE%AE%E6%96%97%E6%95%B8%E6%8E%92%E7%9B%A4+%E2%9C%A8;%E6%BC%94%E7%AE%97%E6%B3%95+%C3%97+%E8%B3%87%E6%96%99%E7%B5%90%E6%A7%8B+%C3%97+%E8%A6%96%E8%A6%BA%E5%8C%96" alt="Typing SVG" />
</p>

<p align="center">
  <a href="#"><img src="https://img.shields.io/badge/%E6%95%99%E5%AD%B8%E5%B0%88%E6%A1%88-Learning%20Project-FF6B6B?style=for-the-badge" alt="教學專案" /></a>
  <a href="#"><img src="https://img.shields.io/badge/Single%20File-HTML%20%2B%20JS%20%2B%20CSS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="Single File" /></a>
  <a href="#"><img src="https://img.shields.io/badge/%E9%9B%A2%E7%B7%9A%E5%8F%AF%E7%94%A8-No%20Server-10B981?style=for-the-badge" alt="離線可用" /></a>
</p>

<p align="center">
  用一個真實的紫微斗數命盤，教你<b>「資料 → 演算法 → 視覺化」</b>的完整旅程<br/>
  <sub>不是 Todo List、不是計算機 — 是一個你會想拿自己生日來玩的專案</sub>
</p>

---

## 這個專案能學到什麼？

```
    ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
    │   資料結構   │ ──→ │   演算法     │ ──→ │   視覺化    │
    │             │     │             │     │             │
    │ 環形陣列     │     │ 查表法       │     │ CSS Grid    │
    │ 嵌套物件     │     │ mod 12 循環  │     │ Container Q │
    │ 2D 矩陣     │     │ 奇偶分支     │     │ DOM 渲染    │
    └─────────────┘     └─────────────┘     └─────────────┘
```

<table>
<tr>
<td width="33%" valign="top">

**演算法**
- `(n % 12 + 12) % 12` 循環索引
- 查表法 O(1) 星曜定位
- 奇偶判斷決定順逆行
- 商餘運算安紫微星

</td>
<td width="33%" valign="top">

**資料結構**
- 12 宮環形陣列
- 每宮 4 層星曜嵌套
- 14 × 12 亮度矩陣
- 10 組四化映射表

</td>
<td width="33%" valign="top">

**視覺化**
- CSS Grid 4×4 命盤佈局
- Container Query 自適應字體
- `position: absolute` 甲級星浮層
- `writing-mode: vertical-rl` 直書

</td>
</tr>
</table>

---

## 三步開始

```
git clone https://github.com/jhihweijhan/zwds-chart.git
```

用瀏覽器打開 `紫微斗數.html`，完成。不需安裝任何東西。

```
zwds-chart/
  ├── 紫微斗數.html       ← 打開這個
  ├── lib/
  │    ├── lunar.js        ← 農曆轉換
  │    └── tailwind.min.js ← CSS 框架
  └── 排盤規則.md          ← 演算法規格書
```

---

## 學習路線圖

```
Level 1 ─ 看懂結構
│
├── HTML：4×4 CSS Grid 怎麼排出十二宮格？
├── CSS：clamp() + cqi 怎麼讓字體自動縮放？
└── JS：chart[0].majors[0].name 資料長什麼樣？

Level 2 ─ 理解演算法
│
├── idx(n) = ((n%12)+12)%12 為什麼要加 12 再取餘？
├── 生日 → 命宮位置：只用一行公式 idx(2 + month - hour)
└── 查表法：SiHua_Table[年干] 直接拿到四化星

Level 3 ─ 理解視覺化
│
├── renderChartDOM() 怎麼把 JS 物件變成 12 個 div？
├── gridArea = "1/2" 怎麼把 div 放到正確的宮格？
└── writing-mode: vertical-rl 怎麼讓星名直書？

Level 4 ─ 動手改
│
├── 改配色（CSS 練習）
├── 加一顆新星（演算法練習）
└── 做一個新互動功能（JS 練習）
```

---

<details>
<summary><b>演算法細節</b>（點擊展開）</summary>

### 核心公式

**循環索引** — 整個系統的基石，用了 50+ 次：
```javascript
idx: n => (n % 12 + 12) % 12
// idx(13) → 1,  idx(-1) → 11,  idx(0) → 0
```

**命宮定位** — 從月份和時辰推算：
```javascript
mingIdx = idx(2 + (month - 1) - hourIdx)   // 逆時針
shenIdx = idx(2 + (month - 1) + hourIdx)   // 順時針
```

**紫微星安法** — 從生日和五行局推算：
```javascript
let q = Math.ceil(day / bureau);
let diff = (q * bureau) - day;
zwIdx = idx(2 + ((diff % 2 !== 0) ? (q - diff) : (q + diff)) - 1);
```

**順逆行判斷** — 陽男陰女順行，陰男陽女逆行：
```javascript
isClock = (yearStem % 2 === 0 && gender === 'M') ||
          (yearStem % 2 !== 0 && gender === 'F');
```

完整排盤規則請參考 [排盤規則.md](排盤規則.md)

</details>

<details>
<summary><b>資料結構細節</b>（點擊展開）</summary>

### 命盤資料模型

每個宮位是一個物件，12 個宮位組成環形陣列：

```javascript
chart[0] = {
    idx: 0,                    // 宮位索引（0-11）
    name: "子",                // 地支
    gan: "甲",                 // 天干
    palace: "命宮",            // 宮位名稱
    majors: [                  // 甲級主星（0-3 顆）
        { name: "紫微", bright: "旺", sihua: "祿" }
    ],
    minors: [...],             // 乙級輔星（0-6 顆）
    minis: [...],              // 丙級雜曜
    minis2: [...],             // 丁級雜曜
    daxian: "2-11",            // 大限（十年運）
    xiaoxian: [1,13,25,...],   // 小限（流年）
    changsheng: "長生",        // 長生十二神
    boshi: "博士"              // 博士十二神
}
```

### 查表資料

| 資料表 | 維度 | 用途 |
|:---|:---|:---|
| `SiHua_Table` | 10 × 4 | 年干 → 四化星名 |
| `Brightness` | 14 × 12 | 主星 × 宮位 → 亮度 |
| `LuCun_Map` | 10 | 年干 → 祿存位置 |
| `KuiYue_Map` | 10 × 2 | 年干 → [魁, 鉞] 位置 |

</details>

<details>
<summary><b>CSS 視覺化技術</b>（點擊展開）</summary>

### 4×4 Grid 命盤佈局

```css
.chart-fullscreen {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
}
```

12 個宮位 + 中宮（2×2），用 `gridArea` 定位：
```
[巳] [午] [未] [申]
[辰] [中] [中] [酉]
[卯] [中] [中] [戌]
[寅] [丑] [子] [亥]
```

### Container Query 自適應

```css
.cell { container-type: inline-size; }

:root {
    --fs-major: clamp(16px, 13cqi, 52px);  /* 隨宮格寬度縮放 */
}
```

`cqi` = 容器寬度的百分比，不是螢幕寬度。宮格小字就小，宮格大字就大。

### 甲級星浮層

```css
.cell-majors {
    position: absolute;     /* 脫離文件流 */
    top: 0; right: 0;
    z-index: 10;            /* 蓋在其他內容上方 */
}
```

甲級星可以延伸超過自己的區域，不會被裁切。

### 直書

```css
.writing-vertical {
    writing-mode: vertical-rl;    /* 從右到左、從上到下 */
    text-orientation: upright;    /* 字元正立 */
}
```

</details>

<details>
<summary><b>JavaScript 設計模式</b>（點擊展開）</summary>

### 雙物件架構

```
ZWDSEngine — 純計算（無 DOM 操作）
    ├── generate()        → 產出 chart 資料物件
    ├── placeStars()      → 安星曜
    ├── calcLimits()      → 算運限
    └── applySiHua()      → 標四化

app — 純 UI（不含命理邏輯）
    ├── showChart()       → 呼叫 Engine，渲染結果
    ├── renderChartDOM()  → 資料 → DOM
    ├── autoSave()        → localStorage 持久化
    └── toggleMiniStars() → UI 開關
```

### DOM 渲染模式

```javascript
// 資料驅動：.map() + 模板字串 + .join()
let html = cell.majors.map(star =>
    `<div class="${star.bright === '旺' ? 'text-red' : 'text-purple'}">
        ${star.name}
    </div>`
).join('');

div.innerHTML = html;  // 一次性注入
```

### 錯誤回報機制

```javascript
try {
    const result = ZWDSEngine.generate(...);
} catch (err) {
    // 彈出確認框，一鍵複製錯誤資訊（含堆疊、瀏覽器版本）
    navigator.clipboard.writeText(errorInfo);
}
```

</details>

---

## 瀏覽器支援

| Chrome | Edge | Firefox | Safari |
|:---:|:---:|:---:|:---:|
| 105+ | 105+ | 110+ | 16+ |

## 授權

MIT License

---

<p align="center">
  <sub>把古老的命理智慧，變成學演算法的最佳教材</sub>
</p>
