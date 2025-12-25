<template>
  <section id="gallery" class="bg-black py-20">
    <div class="mx-auto max-w-6xl px-4 sm:px-6">
      <div class="mb-10 flex flex-col gap-4 sm:flex-row sm:items-end sm:justify-between">
        <div>
          <p class="text-xs tracking-[0.3em] text-white/60">SOCIAL WALL</p>
          <h2 class="mt-3 text-3xl font-black italic tracking-tight">社群互動牆（placeholder）</h2>
        </div>
        <p class="max-w-md text-sm leading-7 text-white/70">
          正方形 Grid；Hover 時圖片縮放並覆蓋紅色透明層，顯示「VIEW」（本次僅 UI）。
        </p>
      </div>

      <div class="grid grid-cols-2 gap-3 sm:grid-cols-3 lg:grid-cols-4">
        <a v-for="i in 8" :key="i" href="#"
          class="group relative aspect-square overflow-hidden rounded-md border border-white/10 bg-zinc-900"
          aria-label="View social item (placeholder)">
          <div
            class="absolute inset-0 bg-[url('https://images.unsplash.com/photo-1521412644187-c49fa049e84d?auto=format&fit=crop&w=800&q=60')] bg-cover bg-center opacity-45 grayscale transition-transform duration-300 group-hover:scale-110" />
          <div class="absolute inset-0 bg-red-500/0 transition-colors duration-300 group-hover:bg-red-500/35" />
          <div class="absolute inset-0 flex items-center justify-center">
            <span
              class="translate-y-1 text-xs font-black tracking-[0.35em] text-white opacity-0 transition-all duration-300 group-hover:translate-y-0 group-hover:opacity-100">
              VIEW
            </span>
          </div>
        </a>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
// Gallery/Social Wall：僅做網格 + hover 視覺效果（不串社群、不做功能）
</script>
