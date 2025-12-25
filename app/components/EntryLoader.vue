<template>
  <!--
    全站進場動畫（規格來自 uiux.md）：
    - 黑色遮罩屏
    - 紅色進度條 0% -> 100%
    - 品牌名稱字母淡入
    - 最後整屏向上滑出

    元件維持結構不變；動畫以 CSS + 少量時序邏輯完成。
  -->
  <div v-if="enabled" class="aolin-entry-root fixed inset-0 z-50 flex items-center justify-center bg-black text-white">
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
        <!-- 進度條：CSS 會用 scaleX 從 0% -> 100% -->
        <div class="aolin-entry-bar h-full w-full bg-red-500" data-entry="bar" />
      </div>

      <p class="mt-4 text-center text-xs tracking-widest text-white/50">LOADING</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const enabled = ref(true)

let t: ReturnType<typeof setTimeout> | null = null
let prevOverflow = ''
let started = false

const start = () => {
  if (started) return
  started = true

  const reduce = window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
  const durationMs = reduce ? 0 : 1600

  prevOverflow = document.documentElement.style.overflow
  document.documentElement.style.overflow = 'hidden'

  t = setTimeout(() => {
    enabled.value = false
    document.documentElement.style.overflow = prevOverflow
  }, durationMs)
}

onMounted(() => {
  const isCssReady = () => document.documentElement?.dataset?.css === 'ready'

  // CSS 已就緒就直接開始；否則等 app/app.vue 發出事件後再開始
  if (isCssReady()) start()
  else window.addEventListener('aolin:css-ready', start, { once: true })
})

onBeforeUnmount(() => {
  if (t) clearTimeout(t)
  t = null
  document.documentElement.style.overflow = prevOverflow || ''
  window.removeEventListener('aolin:css-ready', start as EventListener)
})
</script>
