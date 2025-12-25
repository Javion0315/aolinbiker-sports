# 進場動畫建議（速度感 / 熱血感）

本文件是「建議與範例」，**不改動現有結構**、不強制實作。  
目前掛點已在 `components/EntryLoader.vue` 提供：

- 根層：`.aolin-entry-root`
- 品牌字：`[data-entry="brand"]`
- 副標：`[data-entry="sub"]`
- 進度條：`[data-entry="bar"]`

> 想看效果：可暫時把 `EntryLoader` 的 `enabled = true`（僅用於展示）。

---

## 方案 A：GSAP（最容易做出「速度感」）

### 節奏建議（總長約 1.2s～1.6s）
- **0.00s**：遮罩已在畫面（黑底）
- **0.05s**：品牌字母淡入 + 微下往上（y 18px -> 0）
- **0.12s**：副標跟進（同樣淡入 + 上推）
- **0.18s**：紅色進度條快速衝刺到 100%（帶一點 overshoot）
- **+0.10s**：整屏向上滑出（yPercent: 0 -> -100），乾淨俐落

### Easing 建議
- 文字：`power3.out`（快、乾淨）
- 進度條：`power2.out` 或 `expo.out`（衝刺感）
- 出場：`power4.inOut`（果斷收尾）

### GSAP 範例（直接對現有掛點）

```ts
// components/EntryLoader.vue（僅建議，不一定要放進去）
import gsap from 'gsap'

onMounted(() => {
  const tl = gsap.timeline({ defaults: { ease: 'power3.out' } })

  tl.from('[data-entry="brand"]', { opacity: 0, y: 18, duration: 0.28 })
    .from('[data-entry="sub"]', { opacity: 0, y: 18, duration: 0.28 }, '<0.06')
    // 進度條：用 scaleX 做速度感最直覺
    .fromTo(
      '[data-entry="bar"]',
      { scaleX: 0, transformOrigin: 'left' },
      { scaleX: 1.08, duration: 0.55, ease: 'expo.out' },
      '<0.10'
    )
    // 回彈到 1（讓「衝過頭」更有爆發力）
    .to('[data-entry="bar"]', { scaleX: 1, duration: 0.18, ease: 'power2.out' })
    // 整屏上滑離場
    .to('.aolin-entry-root', { yPercent: -100, duration: 0.50, ease: 'power4.inOut' }, '+=0.06')
})
```

### 「熱血感」加成（仍不改結構）
- **短暫的紅色閃光**：在 `.aolin-entry-root` 加 `box-shadow` 或 `filter: drop-shadow()` 的 tween（0.08～0.12s）。
- **更像轉播的節奏**：讓文字先「進場」再「鎖定」，用 2 段 tween（第一段快、第二段微調）。

---

## 方案 B：純 CSS（不依賴套件，足夠有感）

### 核心做法
用 class 控制動畫，保持 DOM 不動：
- `.aolin-entry-root`：出場上滑
- `[data-entry="brand"]` / `[data-entry="sub"]`：淡入 + 上推
- `[data-entry="bar"]`：scaleX 0 -> 1（可加 overshoot）

### 建議 keyframes（可放在 `app/assets/css/main.css` 末尾）

```css
@keyframes aolin-pop-in {
  0% { opacity: 0; transform: translateY(18px); filter: blur(2px); }
  100% { opacity: 1; transform: translateY(0); filter: blur(0); }
}

@keyframes aolin-bar-sprint {
  0% { transform: scaleX(0); }
  80% { transform: scaleX(1.08); }
  100% { transform: scaleX(1); }
}

@keyframes aolin-curtain-up {
  0% { transform: translateY(0); }
  100% { transform: translateY(-100%); }
}
```

### 套用方式（不改結構，只加 class）
在 `EntryLoader` 根層與元素加上（或用 `:class` 條件套用）：

```css
.aolin-entry-brand { animation: aolin-pop-in 280ms cubic-bezier(.2,.9,.2,1) both; }
.aolin-entry-sub { animation: aolin-pop-in 280ms cubic-bezier(.2,.9,.2,1) both; animation-delay: 60ms; }
.aolin-entry-bar { transform-origin: left; animation: aolin-bar-sprint 700ms cubic-bezier(.2,.9,.2,1) both; animation-delay: 120ms; }
.aolin-entry-root { animation: aolin-curtain-up 520ms cubic-bezier(.2,.9,.2,1) both; animation-delay: 980ms; }
```

> 注意：CSS 方案如果沒有「關閉 enabled」的時序，遮罩會一直存在；所以 CSS 方案通常搭配一個「固定時間後關閉」的狀態，但你目前要求不做完整功能，我們先停在「建議」層級即可。


