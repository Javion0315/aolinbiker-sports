<template>
  <section aria-label="Marquee"
    class="relative z-20 -my-6 overflow-hidden border-y border-white/5 bg-zinc-950 py-10 rotate-[-1.5deg] scale-105 sm:py-12">
    <div class="flex whitespace-nowrap">
      <!-- 無限水平循環：純 CSS linear infinite（熱血速度感） -->
      <div class="aolin-marquee-track flex items-center gap-20 pr-20">
        <div v-for="(word, i) in loopWords" :key="i" class="flex items-center gap-20">
          <span
            class="cursor-default select-none text-6xl font-black italic text-white/10 transition-colors hover:text-red-500/40 md:text-8xl">
            {{ word }}
          </span>
          <div class="h-4 w-4 rotate-45 bg-red-600" aria-hidden="true" />
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
const words = ['800公尺', '樂樂棒球', '躲避球', '地板滾球', '藥球擲遠', '拔河', '體能之巔', '大隊接力'] as const
const loopWords = [...words, ...words]
</script>

<style scoped>
@keyframes aolin-marquee {
  0% {
    transform: translateX(0);
  }

  100% {
    /* 內容複製兩份，移動一半即可無縫循環 */
    transform: translateX(-50%);
  }
}

.aolin-marquee-track {
  width: max-content;
  will-change: transform;
  animation: aolin-marquee 30s linear infinite;
}

@media (prefers-reduced-motion: reduce) {
  .aolin-marquee-track {
    animation: none;
  }
}
</style>
