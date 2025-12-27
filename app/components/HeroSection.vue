<template>
  <section id="hero" ref="rootEl"
    class="aolin-hero relative flex min-h-dvh w-full items-center justify-center overflow-hidden bg-black">
    <!-- Cinematic Video Background（參考：高對比黑白動態影像） -->
    <div class="aolin-hero-bg absolute inset-0 z-0">
      <!-- 注意：影片為 placeholder；實際素材可替換 -->
      <video ref="videoEl" autoplay muted loop playsinline preload="auto"
        class="h-full w-full scale-105 object-cover opacity-70 grayscale"
        style="filter: brightness(0.78) contrast(1.15) grayscale(1)">
        <!-- 使用本機影片：app/assets/video/main.mp4 -->
        <source :src="heroVideoSrc" type="video/mp4" />
        <!-- fallback placeholder（本機影片載入失敗時） -->
        <source src="https://videos.pexels.com/video-files/5192138/5192138-uhd_2560_1440_25fps.mp4" type="video/mp4" />
        Your browser does not support the video tag.
      </video>
      <!-- 黑色遮罩：再降低不透明度，讓影片更清楚 -->
      <div class="absolute inset-0 z-10 bg-gradient-to-b from-black/30 via-transparent to-black/40" />
    </div>

    <!-- Dynamic Four-Color Flares（動畫可用 CSS/GSAP；此處保留 class 與註解） -->
    <div class="pointer-events-none absolute inset-0 z-10 opacity-30 mix-blend-screen">
      <!-- TODO(animation): scale/opacity 循環（速度感/熱血感） -->
      <div
        class="aolin-flare-anim-1 absolute -left-1/4 -top-1/4 h-full w-full rounded-full bg-emerald-600/40 blur-[150px]" />
      <div
        class="aolin-flare-anim-2 absolute -right-1/4 -top-1/4 h-full w-full rounded-full bg-red-600/40 blur-[150px]" />
      <!-- 可選：黃色/藍色 flare（先保留結構） -->
      <div class="absolute -bottom-1/3 -left-1/3 h-full w-full rounded-full bg-yellow-500/25 blur-[170px]" />
      <div class="absolute -bottom-1/3 -right-1/3 h-full w-full rounded-full bg-blue-600/25 blur-[170px]" />
    </div>

    <div class="relative z-20 mx-auto w-full max-w-7xl px-4 text-center sm:px-6">
      <!-- Team Indicators -->
      <div class="aolin-hero-bars mb-8 flex justify-center gap-3">
        <!-- TODO(animation): scaleX 0->1（依序出現） -->
        <span class="aolin-hero-bar aolin-hero-bar-1 h-1.5 w-12 bg-emerald-600 shadow-[0_0_15px_rgba(34,197,94,0.5)]" />
        <span class="aolin-hero-bar aolin-hero-bar-2 h-1.5 w-12 bg-red-600 shadow-[0_0_15px_rgba(239,68,68,0.5)]" />
        <span class="aolin-hero-bar aolin-hero-bar-3 h-1.5 w-12 bg-yellow-500 shadow-[0_0_15px_rgba(234,179,8,0.5)]" />
        <span class="aolin-hero-bar aolin-hero-bar-4 h-1.5 w-12 bg-blue-600 shadow-[0_0_15px_rgba(37,99,235,0.5)]" />
      </div>

      <!-- Title（Overflow hidden + 由下往上推入） -->
      <h1 class="mb-8 mt-4 font-black italic leading-[1.2] tracking-tight text-white drop-shadow-2xl">
        <span class="block text-[9vw] md:text-[6vw]">
          <!-- TODO(animation): y: 100% -> 0 -->
          <span class="aolin-hero-line aolin-hero-line-1 block">團隊挑戰 × 一整年🔥</span>
        </span>
        <span class="block text-[9vw] md:text-[6vw]">
          <!-- TODO(animation): y: 100% -> 0 -->
          <span class="aolin-hero-line aolin-hero-line-2 block aolin-text-stroke">用汗水累積榮耀</span>
        </span>
        <span class="block text-[9vw] md:text-[6vw]">
          <!-- TODO(animation): y: 100% -> 0 -->
          <span class="aolin-hero-line aolin-hero-line-3 block text-red-600">傲臨彼客運動會</span>
        </span>
      </h1>

      <div class="aolin-hero-cta flex flex-col items-center gap-10">
        <p
          class="border border-white/10 bg-black/40 px-6 py-2 text-[10px] font-bold uppercase tracking-[0.4em] text-white backdrop-blur-sm sm:text-xs md:text-sm">
          <span class="font-black text-emerald-400 drop-shadow-[0_0_6px_#059669bb]">綠</span>
          <span class="font-black text-white mx-1">‧</span>
          <span class="font-black text-red-500 drop-shadow-[0_0_6px_#ef4444bb]">紅</span>
          <span class="font-black text-white mx-1">‧</span>
          <span class="font-black text-yellow-400 drop-shadow-[0_0_6px_#facc15bb]">黃</span>
          <span class="font-black text-white mx-1">‧</span>
          <span class="font-black text-blue-400 drop-shadow-[0_0_6px_#60a5fabb]">藍</span>
          <span class="font-black text-white mx-2">|</span>
          <span class="font-black text-white drop-shadow-[0_0_4px_#fff]">四大勢力即將引爆</span>
        </p>

        <div class="flex flex-col gap-6 sm:flex-row sm:items-center sm:justify-center">
          <a href="#cta"
            class="bg-white px-10 py-4 text-2xl font-black italic text-black shadow-2xl transition-all hover:bg-red-600 hover:text-white [transform:skewX(-12deg)] sm:px-16 sm:py-5 sm:text-3xl">
            <span class="inline-block [transform:skewX(12deg)]">JOIN NOW</span>
          </a>

          <a href="#highlights"
            class="group inline-flex items-center gap-4 text-sm font-bold tracking-widest text-white transition-colors hover:text-red-500">
            <span
              class="flex h-14 w-14 items-center justify-center rounded-full border border-white/20 transition-all group-hover:border-red-600">
              <svg class="h-6 w-6 fill-current" viewBox="0 0 24 24" aria-hidden="true">
                <path d="M8 5v14l11-7z" />
              </svg>
            </span>
            WATCH HIGHLIGHTS
          </a>
        </div>
      </div>
    </div>

    <div class="absolute bottom-0 left-0 z-25 h-32 w-full bg-gradient-to-t from-black to-transparent" />
  </section>
</template>

<script setup lang="ts">
// Hero Section：動畫需在進入可視範圍時重新開始（IntersectionObserver + class 觸發）
import { onBeforeUnmount, onMounted, ref } from 'vue'

// 使用 import 讓 Nuxt/Vite 在 SSR/重整時都能產出正確的資產 URL（避免 file:// 或 404）
import heroVideoSrc from '~/assets/video/main.mp4'

const videoEl = ref<HTMLVideoElement | null>(null)
const rootEl = ref<HTMLElement | null>(null)
let io: IntersectionObserver | null = null

onMounted(() => {
  // 有些瀏覽器在重整後仍可能延遲 autoplay，補一個保險 play()
  const v = videoEl.value
  if (!v) return
  v.muted = true
  v.load()
  const tryPlay = () => v.play().catch(() => void 0)
  tryPlay()
  v.addEventListener('canplay', tryPlay, { once: true })

  const reduce = window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
  if (reduce) {
    rootEl.value?.classList.add('aolin-hero--in')
    return
  }

  const restart = () => {
    const el = rootEl.value
    if (!el) return
    el.classList.remove('aolin-hero--in')
    void el.offsetWidth
    el.classList.add('aolin-hero--in')
  }

  io = new IntersectionObserver(
    (entries) => {
      const e = entries[0]
      if (!e) return
      if (e.isIntersecting) restart()
    },
    { threshold: 0.35 }
  )

  if (rootEl.value) io.observe(rootEl.value)
})
onBeforeUnmount(() => {
  io?.disconnect()
  io = null
})
</script>
