<template>
  <!--
    全站進場動畫（穩定版）：
    - DOM 常駐（避免 iOS 殘影）
    - opacity 淡出 + pointer-events none
    - JS 僅負責時序，不控制動畫細節
  -->
  <div class="aolin-entry-root fixed inset-0 z-50 flex items-center justify-center bg-black text-white
           transition-opacity duration-500" :class="{ 'opacity-0 pointer-events-none': !enabled }">
    <div class="w-full max-w-lg px-8">
      <div class="mb-8 text-center">
        <p class="aolin-entry-brand text-xs tracking-[0.35em] text-white/70" data-entry="brand">
          傲臨彼客全民運動會
        </p>
        <h1 class="aolin-entry-sub mt-3 text-4xl font-black italic tracking-tight" data-entry="sub">
          SPORTS
        </h1>
      </div>

      <div class="h-2 w-full overflow-hidden bg-white/10">
        <div class="aolin-entry-bar h-full w-full bg-red-500" data-entry="bar" />
      </div>

      <p class="mt-4 text-center text-xs tracking-widest text-white/50">
        LOADING
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue'

/** ===== 狀態 ===== */
const enabled = ref(true)

let t: ReturnType<typeof setTimeout> | null = null
let prevOverflow = ''
let started = false

/** ===== 環境判斷 ===== */
const isInApp =
  typeof navigator !== 'undefined' &&
  /FBAN|FBAV|Instagram|Line/i.test(navigator.userAgent)

/** ===== 關閉（iOS 安全） ===== */
const hide = () => {
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      enabled.value = false
      document.documentElement.style.overflow = prevOverflow

      // In-App Browser 強制 reflow（避免殘影）
      if (isInApp) {
        document.body.style.display = 'none'
        // eslint-disable-next-line @typescript-eslint/no-unused-expressions
        document.body.offsetHeight
        document.body.style.display = ''
      }
    })
  })
}

/** ===== 啟動 ===== */
const start = () => {
  if (started) return
  started = true

  const reduce =
    window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false

  const durationMs = reduce ? 0 : 1600

  prevOverflow = document.documentElement.style.overflow
  document.documentElement.style.overflow = 'hidden'

  t = setTimeout(hide, durationMs)

  // 🛟 In-App Browser 保命（一定關）
  if (isInApp) {
    setTimeout(hide, 3000)
  }
}

onMounted(() => {
  const isCssReady = () =>
    document.documentElement?.dataset?.css === 'ready'

  if (isCssReady()) start()
  else window.addEventListener('aolin:css-ready', start, { once: true })

  // 🧩 WebView / Safari 補救
  window.addEventListener('pageshow', hide, { once: true })
  document.addEventListener('visibilitychange', () => {
    if (document.visibilityState === 'visible') hide()
  })
})

onBeforeUnmount(() => {
  if (t) clearTimeout(t)
  t = null
  document.documentElement.style.overflow = prevOverflow || ''
  window.removeEventListener('aolin:css-ready', start as EventListener)
})
</script>
