<template>
  <section id="spirit" ref="rootEl"
    class="aolin-spirit relative overflow-hidden bg-white pb-20 pt-24 text-black sm:pt-28 md:pt-32">
    <!-- Top Transition Gradient（黑 -> 透明） -->
    <div class="pointer-events-none absolute left-0 top-0 h-24 w-full bg-gradient-to-b from-black to-transparent" />

    <div class="relative z-10 mx-auto max-w-7xl px-4 sm:px-6">
      <div class="mb-20 grid grid-cols-1 gap-12 lg:mb-24 lg:grid-cols-2 lg:items-center lg:gap-16">
        <!-- Left: copy -->
        <div>
          <!-- TODO(animation): 進入視窗時淡入 + x 位移 -->
          <div class="aolin-spirit-kicker mb-6 flex items-center gap-4">
            <div class="h-[2px] w-12 bg-red-600" />
            <span class="text-xs font-black uppercase tracking-[0.3em] text-red-600">Core Philosophy</span>
          </div>

          <!-- TODO(animation): 大標由下往上推入 -->
          <h2
            class="aolin-spirit-title mb-10 text-7xl font-black italic leading-[0.8] tracking-tight sm:text-8xl md:text-9xl">
            OUR<br />
            <span class="text-red-600">SPIRIT</span>
          </h2>

          <p class="aolin-spirit-copy max-w-xl text-lg font-bold leading-relaxed text-zinc-800 sm:text-xl md:text-2xl">
            我們不只是在主辦賽事，我們在創造記憶。在每一滴汗水中，看見人類最原始的力量與韌性。
          </p>

          <div class="aolin-spirit-tags mt-10 flex flex-wrap gap-3">
            <span v-for="tag in tags" :key="tag"
              class="bg-black px-5 py-1 text-lg font-black italic text-white [transform:skewX(-12deg)]">
              <span class="inline-block [transform:skewX(12deg)]">#{{ tag }}</span>
            </span>
          </div>
        </div>

        <!-- Right: image + callout -->
        <div class="aolin-spirit-card relative aspect-[4/5] md:aspect-square">
          <div class="absolute inset-0 z-0 bg-red-600 [transform:rotate(-3deg)]" />
          <img :src="spiritImageSrc" alt="Spirit action"
            class="absolute inset-0 z-10 h-full w-full object-cover shadow-2xl transition-all duration-700 hover:grayscale-0" />

          <!-- Parallax Callout（用最小 JS 做 x 位移；符合 uiux 的 parallax） -->
          <div
            class="absolute -bottom-6 -right-6 z-20 bg-black p-6 text-3xl font-black italic text-white shadow-2xl [transform:skewX(-6deg)] sm:p-8 md:p-10 md:text-4xl"
            :style="{ transform: `skewX(-6deg) translateX(${xRight}px)` }">
            <span class="inline-block [transform:skewX(6deg)]">PUSH YOUR LIMITS.</span>
          </div>
        </div>
      </div>

      <!-- Stats -->
      <div class="grid grid-cols-2 gap-10 border-t-2 border-black/5 py-16 lg:grid-cols-4 lg:gap-12">
        <div v-for="stat in stats" :key="stat.label" class="aolin-spirit-stat group text-center lg:text-left">
          <!-- TODO(animation): 進入視窗淡入上推；數字可做遞增動畫 -->
          <div
            class="text-6xl font-black italic leading-none transition-colors duration-300 group-hover:text-red-600 sm:text-7xl md:text-8xl">
            {{ stat.value }}
          </div>
          <div class="mt-2 text-[10px] font-black uppercase tracking-[0.5em] text-zinc-400">
            {{ stat.label }}
          </div>
        </div>
      </div>
    </div>

    <!-- Subtle Texture for Depth -->
    <div
      class="pointer-events-none absolute inset-0 opacity-[0.03] mix-blend-multiply bg-[url('https://www.transparenttextures.com/patterns/stardust.png')]" />
  </section>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const tags = ['COMMUNITY', 'POWER', 'REALITY', 'NO_LIMITS'] as const

const stats = [
  { label: 'ATHLETES', value: '200+' },
  { label: 'SPORTS', value: '8+' },
  { label: 'TEAMS', value: '4' },
  { label: 'SWEAT (LITERS)', value: '∞' }
] as const

// Spirit 圖片：從 app/assets/img/spirit/ 隨機挑一張
// SSR 先用第一張，避免 hydration mismatch；mounted 後再 random
const spiritImages = Object.values(
  import.meta.glob('../assets/img/spirit/*.{jpg,jpeg,png,webp,JPG,JPEG,PNG,WEBP}', {
    eager: true,
    import: 'default'
  })
) as string[]

const currentIdx = ref(0)
const spiritImageSrc = ref<string>(spiritImages[0] || '')

// 視差：讓右下角標語塊隨滾動向右滑（最小實作；可視需求移除只留註解）
const rootEl = ref<HTMLElement | null>(null)
const xRight = ref(0)
let raf = 0
let intervalId: ReturnType<typeof setInterval> | null = null
let io: IntersectionObserver | null = null

const restart = () => {
  const el = rootEl.value
  if (!el) return
  el.classList.remove('aolin-spirit--in')
  void el.offsetWidth
  el.classList.add('aolin-spirit--in')
}

const pickRandomNext = () => {
  if (spiritImages.length === 0) return
  if (spiritImages.length === 1) {
    currentIdx.value = 0
    spiritImageSrc.value = spiritImages[0]!
    return
  }

  let next = currentIdx.value
  // 避免連續抽到同一張
  while (next === currentIdx.value) {
    next = Math.floor(Math.random() * spiritImages.length)
  }
  currentIdx.value = next
  spiritImageSrc.value = spiritImages[next]!
}

const update = () => {
  const el = rootEl.value
  if (!el) return

  const rect = el.getBoundingClientRect()
  const vh = window.innerHeight || 1
  // 參考 [0.4, 0.8] → [0, 150]：用區塊進入視窗的進度做映射
  const progress = (vh - rect.top) / (vh + rect.height)
  const t = Math.min(1, Math.max(0, (progress - 0.4) / 0.4))
  xRight.value = Math.round(t * 150)
}

onMounted(() => {
  const reduce = window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
  if (reduce) rootEl.value?.classList.add('aolin-spirit--in')

  // 進入視窗播放一次的進場動畫
  io = new IntersectionObserver(
    (entries) => {
      const e = entries[0]
      if (!e) return
      if (e.isIntersecting) restart()
    },
    { threshold: 0.25 }
  )
  if (rootEl.value) io.observe(rootEl.value)

  // 初次隨機（client-only）
  pickRandomNext()

  // 每 3 秒換一張（random，不連續重複）
  intervalId = window.setInterval(pickRandomNext, 3000)

  const onScroll = () => {
    if (raf) return
    raf = window.requestAnimationFrame(() => {
      raf = 0
      update()
    })
  }

  update()
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('resize', onScroll)

  onBeforeUnmount(() => {
    window.removeEventListener('scroll', onScroll)
    window.removeEventListener('resize', onScroll)
  })
})

onBeforeUnmount(() => {
  io?.disconnect()
  io = null
  if (intervalId) clearInterval(intervalId)
  intervalId = null
  if (raf) window.cancelAnimationFrame(raf)
  raf = 0
})
</script>
