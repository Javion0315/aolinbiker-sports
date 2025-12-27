<template>
  <section id="gallery" class="relative overflow-hidden border-t border-white/5 bg-black py-32">
    <!-- Background Decorative Marquee Text -->
    <div
      class="pointer-events-none absolute left-0 top-1/2 z-0 -translate-y-1/2 w-full select-none overflow-hidden opacity-[0.03]">
      <div class="aolin-gallery-marquee flex whitespace-nowrap font-black italic leading-none text-[20vw]">
        <span class="mr-20">MOMENTS OF GLORY</span>
        <span class="mr-20">MOMENTS OF GLORY</span>
        <span class="mr-20">MOMENTS OF GLORY</span>
      </div>
    </div>

    <div class="relative z-10 mx-auto max-w-7xl px-6">
      <!-- Header Section -->
      <div class="mb-16 flex flex-col gap-6 md:flex-row md:items-end md:justify-between">
        <div>
          <div class="mb-4 flex items-center gap-4">
            <div class="h-[2px] w-12 bg-red-600" />
            <span class="font-black uppercase tracking-[0.4em] text-xs text-red-500">Social Community</span>
          </div>
          <h2 class="font-black italic leading-none text-6xl md:text-8xl">
            MOMENTS OF <span class="text-red-600">GLORY</span>
          </h2>
          <p class="mt-4 text-xs font-bold uppercase tracking-[0.2em] text-zinc-500">
            FOLLOW THE HEAT @AOLINBIKER.SPORTS
          </p>
        </div>
        <div class="flex gap-4">
          <a href="https://www.instagram.com/aolinbiker.sports/"
            class="border border-white/10 bg-white/5 px-6 py-2 font-black italic text-lg transition-colors hover:bg-red-600 [transform:skewX(-12deg)]">
            <span class="inline-block [transform:skewX(12deg)]">INSTAGRAM</span>
          </a>
          <a href="https://www.threads.com/@aolinbiker.sports"
            class="border border-white/10 bg-white/5 px-6 py-2 font-black italic text-lg transition-colors hover:bg-red-600 [transform:skewX(-12deg)]">
            <span class="inline-block [transform:skewX(12deg)]">THREADS</span>
          </a>
        </div>
      </div>

      <!-- Infinite Scrolling Gallery -->
      <div class="relative z-10">
        <div class="flex overflow-hidden" style="contain: layout style paint;">
          <div class="aolin-gallery-scroll flex flex-nowrap gap-4" :style="{ '--items-count': socialPosts.length }">
            <div v-for="(post, idx) in scrollPosts" :key="`${post.id}-${idx}`"
              class="group relative aspect-square w-[280px] flex-shrink-0 overflow-hidden border border-white/5 bg-zinc-900 md:w-[400px]">
              <img :src="post.img" :alt="`Glory Moment ${idx + 1}`"
                class="aolin-gallery-img h-full w-full object-cover transition-transform duration-700 group-hover:scale-110 group-hover:grayscale-0"
                loading="lazy" decoding="async" />

              <!-- Hover Overlay -->
              <div
                class="absolute inset-0 flex flex-col items-center justify-center bg-red-600/40 opacity-0 backdrop-blur-[2px] transition-opacity duration-300 group-hover:opacity-100"
                style="will-change: opacity; transform: translateZ(0);">
                <span
                  class="mb-2 font-black italic text-5xl text-white transition-transform duration-300 group-hover:translate-y-0 translate-y-4"
                  style="will-change: transform;">
                  VIEW
                </span>
                <div class="h-1 w-12 bg-white transition-transform duration-300 group-hover:scale-x-100 scale-x-0"
                  style="will-change: transform;" />
              </div>

              <!-- Platform Badge -->
              <!-- <div class="absolute bottom-4 left-4 z-20">
                <span
                  class="border border-white/10 bg-black/60 px-2 py-1 text-[8px] font-black uppercase tracking-widest text-white backdrop-blur-md">
                  {{ post.platform }}
                </span>
              </div> -->

              <!-- Decorative Scanline Effect -->
              <div
                class="pointer-events-none absolute inset-0 bg-[linear-gradient(rgba(18,16,16,0)_50%,rgba(0,0,0,0.25)_50%),linear-gradient(90deg,rgba(255,0,0,0.06),rgba(0,255,0,0.02),rgba(0,0,255,0.06))] bg-[length:100%_2px,3px_100%] opacity-20 transition-opacity group-hover:opacity-0" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Bottom Border Accent -->
    <div
      class="absolute bottom-0 left-0 h-[1px] w-full bg-gradient-to-r from-transparent via-white/10 to-transparent" />
  </section>
</template>

<script setup lang="ts">
// 動態導入所有圖片
const galleryImages = Object.values(
  import.meta.glob('../assets/img/gallery/*.{jpg,jpeg,png,webp,JPG,JPEG,PNG,WEBP}', {
    eager: true,
    import: 'default'
  })
) as string[]

// 創建帖子數據（輪流分配平台）
const platforms = ['ig', 'threads'] as const
const socialPosts = galleryImages.map((img, idx) => ({
  id: String(idx + 1),
  platform: platforms[idx % platforms.length],
  img
}))

// 將資料加倍以實現無縫滾動（3份）
const scrollPosts = [...socialPosts, ...socialPosts, ...socialPosts]
</script>

<style scoped>
/* 背景裝飾文字滾動動畫 */
@keyframes aolin-gallery-marquee {
  0% {
    transform: translate3d(0, 0, 0);
  }

  100% {
    transform: translate3d(-1000px, 0, 0);
  }
}

.aolin-gallery-marquee {
  animation: aolin-gallery-marquee 50s linear infinite;
  will-change: transform;
  backface-visibility: hidden;
  transform: translate3d(0, 0, 0);
  contain: layout style paint;
}

/* 無限滾動畫廊動畫 */
/* 手機版：每張 280px + gap 16px */
@keyframes aolin-gallery-scroll-mobile {
  0% {
    transform: translate3d(0, 0, 0);
  }

  100% {
    /* 移動一組的距離（使用 CSS 變數） */
    transform: translate3d(calc(-1 * (280px + 16px) * var(--items-count, 16)), 0, 0);
  }
}

/* 桌面版：每張 400px + gap 16px */
@keyframes aolin-gallery-scroll-desktop {
  0% {
    transform: translate3d(0, 0, 0);
  }

  100% {
    transform: translate3d(calc(-1 * (400px + 16px) * var(--items-count, 16)), 0, 0);
  }
}

.aolin-gallery-scroll {
  animation: aolin-gallery-scroll-mobile 120s linear infinite;
  will-change: transform;
  backface-visibility: hidden;
  perspective: 1000px;
  transform: translate3d(0, 0, 0);
  contain: layout style paint;
  isolation: isolate;
}

.aolin-gallery-scroll:hover {
  animation-duration: 200s;
}

@media (min-width: 768px) {
  .aolin-gallery-scroll {
    animation-name: aolin-gallery-scroll-desktop;
  }
}

/* 圖片性能優化 */
.aolin-gallery-img {
  transform: translateZ(0);
  backface-visibility: hidden;
  -webkit-font-smoothing: antialiased;
  will-change: transform, filter;
  contain: layout style paint;
  pointer-events: none;
}

/* 卡片性能優化 */
.aolin-gallery-scroll>div {
  transform: translateZ(0);
  backface-visibility: hidden;
  will-change: transform;
  contain: layout style paint;
  isolation: isolate;
}

@media (prefers-reduced-motion: reduce) {

  .aolin-gallery-marquee,
  .aolin-gallery-scroll {
    animation: none;
  }
}
</style>
