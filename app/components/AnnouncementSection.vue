<template>
  <section id="announcement" ref="rootEl" class="aolin-ann relative z-30 overflow-hidden bg-black py-20">
    <div class="mx-auto max-w-7xl px-4 sm:px-6">
      <div
        class="relative overflow-hidden border border-white/10 bg-zinc-950 p-6 shadow-[0_0_50px_rgba(0,0,0,0.5)] md:p-12">
        <!-- Poster-like Decorations -->
        <!-- <div
          class="pointer-events-none absolute left-0 top-0 h-28 w-28 bg-red-600/70 [clip-path:polygon(0_0,100%_0,0_100%)]" /> -->
        <div class="pointer-events-none absolute bottom-0 -left-2 h-16 w-1/3 bg-red-600/20 [transform:skewX(-12deg)]" />
        <div
          class="pointer-events-none absolute right-0 top-0 h-full w-1/2 bg-[radial-gradient(circle_at_80%_10%,rgba(239,68,68,0.18),transparent_55%)]" />
        <div class="pointer-events-none absolute right-8 top-8 hidden grid-cols-6 gap-2 opacity-25 lg:grid">
          <div v-for="i in 24" :key="i" class="h-1 w-1 rounded-full bg-white" />
        </div>

        <!-- Header Bar -->
        <div
          class="aolin-ann-anim relative z-20 mb-8 flex flex-col items-center justify-between gap-4 border-b border-white/10 pb-6 md:mb-12 md:flex-row">
          <div class="bg-red-600 px-6 py-2 [transform:skewX(-12deg)]">
            <span class="block text-sm font-bold italic tracking-[0.2em] text-white [transform:skewX(12deg)]">
              傲臨彼客 全民運動會
            </span>
          </div>

          <div class="text-center md:text-right">
            <p class="mb-1 text-xs font-bold uppercase tracking-widest text-zinc-400">
              參賽費用：{{ current.cost }}
            </p>
            <p class="text-[10px] font-bold italic tracking-tight text-zinc-600">
              {{ current.note }}
            </p>
          </div>

          <div class="hidden border border-white/10 bg-white/5 px-6 py-2 text-right [transform:skewX(-12deg)] lg:block">
            <div class="[transform:skewX(12deg)]">
              <p class="text-xs font-black uppercase tracking-widest text-red-500">{{ current.location }}</p>
              <p class="mt-1 text-xl font-black italic leading-none text-white">{{ current.time }}</p>
            </div>
          </div>
        </div>

        <!-- Mobile Location/Time -->
        <div
          class="aolin-ann-anim mb-10 flex items-center justify-between gap-4 border border-white/10 bg-white/5 px-4 py-3 lg:hidden">
          <div>
            <p class="text-[10px] font-black uppercase tracking-[0.35em] text-white/60">LOCATION</p>
            <p class="mt-1 text-sm font-bold text-red-400">{{ current.location }}</p>
          </div>
          <div class="text-right">
            <p class="text-[10px] font-black uppercase tracking-[0.35em] text-white/60">MEET</p>
            <p class="mt-1 text-sm font-black italic text-white">{{ current.time }}</p>
          </div>
        </div>

        <!-- Content -->
        <Transition name="aolin-fade-up" mode="out-in">
          <div :key="current.id" class="aolin-ann-anim grid grid-cols-1 items-center gap-10 lg:grid-cols-12 lg:gap-12">
            <!-- Hexagon Gallery Side（只做電腦版：lg+，手機直接拿掉避免跑版） -->
            <div class="relative hidden min-w-0 items-center justify-center lg:flex lg:col-span-5">
              <!-- 電腦版舞台：跟著欄寬縮放，避免 480px 固定寬造成溢出 -->
              <div class="relative aspect-square w-full max-w-[460px]">
                <!-- 三個同大小六角形：蜂巢式堆疊（固定比例定位，不跑版） -->
                <HexagonImage :src="current.images[0]"
                  wrapper-class="absolute left-1/2 -top-16 z-30 h-[clamp(120px,15vw,200px)] w-[clamp(120px,15vw,200px)] -translate-x-1/2 border-4 border-black" />

                <div class="absolute left-[27%] top-[45%] z-20 flex gap-2">
                  <HexagonImage :src="current.images[1]"
                    wrapper-class="h-[clamp(120px,15vw,200px)] w-[clamp(120px,15vw,200px)] -translate-x-1/2 -translate-y-1/2" />
                  <HexagonImage :src="current.images[3]"
                    wrapper-class="h-[clamp(120px,15vw,200px)] w-[clamp(120px,15vw,200px)] -translate-x-1/2 -translate-y-1/2" />
                </div>

                <HexagonImage :src="current.images[2]"
                  wrapper-class="absolute left-1/2 top-[36%] z-30 h-[clamp(120px,15vw,200px)] w-[clamp(120px,15vw,200px)] -translate-x-1/2 -translate-y-1/2 " />
              </div>
            </div>

            <!-- Text Details Side（保留右半邊 12 度斜切） -->
            <div class="relative z-20 lg:col-span-7">
              <!-- Huge season number (poster feel) -->
              <div
                class="pointer-events-none absolute -right-2 -top-6 select-none text-[120px] font-black italic text-white/5 sm:text-[160px]">
                {{ current.seasonNo }}
              </div>

              <div class="[transform:skewX(-12deg)]">
                <div class="[transform:skewX(12deg)]">
                  <h2 class="mb-6 text-6xl font-black italic leading-[0.82] tracking-tight text-white md:text-[5.5rem]">
                    <span class="text-white/70">{{ current.season }}</span><br />
                    <span class="drop-shadow-[0_12px_12px_rgba(0,0,0,0.55)]">{{ current.title }}</span>
                  </h2>

                  <div class="mb-10 flex items-start gap-4">
                    <div class="mt-1 bg-red-600 p-1 [transform:skewX(-12deg)]">
                      <svg class="h-5 w-5 [transform:skewX(12deg)]" fill="currentColor" viewBox="0 0 20 20"
                        aria-hidden="true">
                        <path fill-rule="evenodd"
                          d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                          clip-rule="evenodd" />
                      </svg>
                    </div>
                    <div>
                      <p class="text-2xl font-black uppercase leading-none tracking-tight text-yellow-500 md:text-3xl">
                        {{ current.date }}
                      </p>
                      <p class="mt-1 text-sm font-bold uppercase italic tracking-[0.2em] text-white/40">
                        {{ current.slogan }}
                      </p>
                    </div>
                  </div>

                  <div class="mb-10 border-l-4 border-red-600 bg-white/5 p-6 shadow-[0_0_40px_rgba(0,0,0,0.35)]">
                    <h4
                      class="mb-4 flex items-center gap-2 text-[10px] font-black uppercase tracking-[0.5em] text-zinc-500">
                      比賽項目
                    </h4>
                    <div class="flex flex-wrap gap-x-6 gap-y-3">
                      <span v-for="(item, i) in current.items" :key="i"
                        class="cursor-default whitespace-nowrap text-base font-bold text-zinc-300 transition-colors hover:text-red-500 md:text-xl">
                        {{ item }}
                      </span>
                    </div>
                  </div>

                  <div class="flex items-center gap-4">
                    <div class="h-[1px] w-10 bg-red-600" />
                    <p class="max-w-lg text-sm font-medium italic leading-relaxed text-zinc-500">
                      {{ current.season }}將考驗速度、力量、協作與耐力 💪<br />
                      每一場比賽，都是翻轉積分的關鍵！一起全力以赴！
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </Transition>

        <!-- Slider Controls -->
        <div class="absolute bottom-8 right-8 z-40 flex gap-3">
          <!-- TODO(animation): 切換時文字/圖片位移 + 淡入淡出 -->
          <button type="button" @click="prevSlide"
            class="flex h-12 w-12 items-center justify-center border border-white/10 bg-black/50 text-white backdrop-blur-md transition-all hover:border-red-600 hover:bg-red-600 [transform:skewX(-12deg)] sm:h-14 sm:w-14"
            aria-label="Previous">
            <!-- Prev：左箭頭 -->
            <svg class="h-6 w-6 [transform:skewX(12deg)]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
            </svg>
          </button>
          <button type="button" @click="nextSlide"
            class="flex h-12 w-12 items-center justify-center border border-white/10 bg-black/50 text-white backdrop-blur-md transition-all hover:border-red-600 hover:bg-red-600 [transform:skewX(-12deg)] sm:h-14 sm:w-14"
            aria-label="Next">
            <svg class="h-6 w-6 [transform:skewX(12deg)]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </button>
        </div>

        <!-- Pagination Indicators -->
        <div class="absolute bottom-6 left-1/2 z-40 flex -translate-x-1/2 gap-3">
          <button v-for="(_, i) in announcements" :key="i" type="button" @click="setIndex(i)"
            class="h-1.5 rounded-full transition-all duration-500"
            :class="currentIndex === i ? 'w-12 bg-red-600' : 'w-4 bg-zinc-800 hover:bg-zinc-700'"
            :aria-label="`Go to slide ${i + 1}`" />
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

// 圖片暫用：assets/img/spirit/*
const spiritImages = Object.values(
  import.meta.glob('../assets/img/spirit/*.{jpg,jpeg,png,webp,JPG,JPEG,PNG,WEBP}', {
    eager: true,
    import: 'default'
  })
) as string[]

type ImageSet = [string, string, string, string]

const pickImages = (start: number): ImageSet => {
  const safe = (i: number) => spiritImages[i % spiritImages.length] || spiritImages[0] || ''
  return [safe(start), safe(start + 1), safe(start + 2), safe(start + 3)]
}

const announcements = [
  {
    id: '1',
    season: '第四季',
    seasonNo: '04',
    title: '即將開戰！',
    date: '12/14 (日) 隆重登場',
    slogan: '燃燒鬥志，迎接寒冬！',
    items: ['拔河', '足壘球', '飛盤爭奪', '體能之巔', '地板滾球', '藥球擲遠', '大隊接力', '躲避球（新賽制）', '4*200公尺男女混合接力'],
    cost: '每人 200 元',
    note: '(費用包含場地租借、保險、瓶裝水、器材租借..等等)',
    location: '雙蓮國小田徑場',
    time: '集合時間 08:30',
    images: pickImages(1)
  },
  {
    id: '2',
    season: '第三賽季',
    seasonNo: '03',
    title: '即將開戰！',
    date: '10/19 (日) 全員備戰！',
    slogan: '挑戰極限、團隊出擊！',
    items: ['800公尺', '樂樂棒球', '躲避球', '地板滾球', '藥球擲遠', '拔河', '六人七腳', '大隊接力', '體能之巔'],
    cost: '每人 200 元',
    note: '(費用包含場地租借、保險、瓶裝水、器材租借..等等)',
    location: '雙蓮國小田徑場',
    time: '集合時間 08:30',
    images: pickImages(0)
  },
] as const

const currentIndex = ref(0)
const current = computed(() => announcements[currentIndex.value]!)

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % announcements.length
}
const prevSlide = () => {
  currentIndex.value = (currentIndex.value - 1 + announcements.length) % announcements.length
}
const setIndex = (i: number) => {
  currentIndex.value = Math.max(0, Math.min(i, announcements.length - 1))
}

// 進入可視範圍就重播進場（符合全站規則）
const rootEl = ref<HTMLElement | null>(null)
let io: IntersectionObserver | null = null

const restart = () => {
  const el = rootEl.value
  if (!el) return
  el.classList.remove('aolin-ann--in')
  void el.offsetWidth
  el.classList.add('aolin-ann--in')
}

onMounted(() => {
  const reduce = window.matchMedia?.('(prefers-reduced-motion: reduce)')?.matches ?? false
  if (reduce) {
    rootEl.value?.classList.add('aolin-ann--in')
    return
  }

  io = new IntersectionObserver(
    (entries) => {
      const e = entries[0]
      if (!e) return
      if (e.isIntersecting) restart()
      else rootEl.value?.classList.remove('aolin-ann--in')
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

<style scoped>
.aolin-ann .aolin-ann-anim {
  opacity: 0;
  transform: translateY(16px);
}

.aolin-ann.aolin-ann--in .aolin-ann-anim {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 520ms cubic-bezier(0.16, 1, 0.3, 1), transform 520ms cubic-bezier(0.16, 1, 0.3, 1);
}

.aolin-ann.aolin-ann--in .aolin-ann-anim:nth-of-type(1) {
  transition-delay: 80ms;
}

.aolin-ann.aolin-ann--in .aolin-ann-anim:nth-of-type(2) {
  transition-delay: 160ms;
}

.aolin-ann.aolin-ann--in .aolin-ann-anim:nth-of-type(3) {
  transition-delay: 240ms;
}

.aolin-fade-up-enter-active,
.aolin-fade-up-leave-active {
  transition: opacity 600ms cubic-bezier(0.22, 1, 0.36, 1), transform 600ms cubic-bezier(0.22, 1, 0.36, 1);
}

.aolin-fade-up-enter-from,
.aolin-fade-up-leave-to {
  opacity: 0;
  transform: translateY(22px);
}
</style>
