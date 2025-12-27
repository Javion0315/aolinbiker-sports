<template>
  <section id="cta" ref="rootEl" class="aolin-cta relative overflow-hidden bg-red-600 py-28 sm:py-32 md:py-40">
    <!-- Background Graphic（超大裝飾字） -->
    <div class="pointer-events-none absolute right-0 top-0 h-full w-1/2 opacity-10">
      <h2 class="aolin-cta-go select-none text-[40vw] font-black leading-none text-black">GO</h2>
    </div>

    <div class="relative z-10 mx-auto max-w-7xl px-4 text-center sm:px-6">
      <div class="mx-auto max-w-3xl">
        <h2
          class="text-balance text-2xl font-black italic tracking-tight text-white leading-[1.5] sm:text-3xl md:text-4xl">
          <div class="aolin-cta-line sm:whitespace-nowrap leading-[1.4]">一場為期一年的熱血征戰</div>
          <div class="aolin-cta-line sm:whitespace-nowrap leading-[1.4]">一場考驗團隊合作與個人巔峰的競技盛事</div>
          <div class="aolin-cta-line text-black sm:whitespace-nowrap leading-[1.4]">我們要的不只是選手，我們要的是——真正的戰士！</div>
        </h2>

        <p
          class="aolin-cta-copy mx-auto mt-10 max-w-2xl text-sm font-bold leading-[1.5] text-white/80 sm:text-base md:text-xl">
          不管你是天生神力，還是拼勁十足，這場運動會都缺你不可！<br />
          快來報名，和我們一起創造榮耀時刻！
        </p>

        <div class="mt-14 flex justify-center">
          <a href="https://docs.google.com/forms/d/e/1FAIpQLSevLIo0wxuPcNoUNaXBoAQGcumQo-HBCB9OKiTuq5GXQOoPAg/viewform?usp=send_form"
            class="aolin-cta-btn inline-flex items-center justify-center bg-black px-10 py-5 text-2xl font-black italic text-white shadow-2xl transition-all hover:bg-white hover:text-red-600 sm:px-16 sm:py-6 sm:text-4xl">
            JOIN NOW
          </a>
        </div>
      </div>
    </div>

    <div
      class="aolin-cta-tags pointer-events-none absolute bottom-8 left-0 flex w-full justify-center gap-6 text-black/30 sm:bottom-10 sm:gap-20 whitespace-nowrap">
      <span class="text-lg font-black italic tracking-widest sm:text-2xl">#影響綠</span>
      <span class="text-lg font-black italic tracking-widest sm:text-2xl">#紅勝</span>
      <span class="text-lg font-black italic tracking-widest sm:text-2xl">#大黃蜂</span>
      <span class="text-lg font-black italic tracking-widest sm:text-2xl">#藍不住</span>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

// CTA：熱血進場動畫（每次進入可視範圍都重新開始）
const rootEl = ref<HTMLElement | null>(null)
let io: IntersectionObserver | null = null

const restart = () => {
  const el = rootEl.value
  if (!el) return
  el.classList.remove('aolin-cta--in')
  // 強制 reflow，確保 animation 重新開始
  void el.offsetWidth
  el.classList.add('aolin-cta--in')
}

onMounted(() => {
  const reduce = window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
  if (reduce) {
    rootEl.value?.classList.add('aolin-cta--in')
    return
  }

  io = new IntersectionObserver(
    (entries) => {
      const e = entries[0]
      if (!e) return
      if (e.isIntersecting) restart()
    },
    { threshold: 0.25 }
  )

  if (rootEl.value) io.observe(rootEl.value)
})

onBeforeUnmount(() => {
  io?.disconnect()
  io = null
})
</script>
