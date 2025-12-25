<template>
  <section id="highlights" class="bg-black py-20">
    <div class="mx-auto max-w-6xl px-4 sm:px-6">
      <div class="mb-10 flex flex-col gap-4 sm:flex-row sm:items-end sm:justify-between">
        <div>
          <p class="text-xs tracking-[0.3em] text-white/60">EVENT HIGHLIGHT</p>
          <h2 class="mt-3 text-3xl font-black italic tracking-tight">賽事月曆（placeholder）</h2>
        </div>
        <p class="max-w-md text-sm leading-7 text-white/70">
          以月曆/卡片方式呈現近期賽事與亮點（此處僅 UI 結構）。
        </p>
      </div>

      <div class="grid gap-6 lg:grid-cols-12">
        <div class="lg:col-span-7">
          <div class="rounded-lg border border-white/10 bg-zinc-950/70 p-6">
            <div class="flex items-center justify-between">
              <p class="text-sm font-semibold tracking-wide text-white">MARCH 2026</p>
              <div class="flex gap-2">
                <button type="button"
                  class="border border-white/15 bg-white/5 px-3 py-1.5 text-xs font-semibold tracking-widest text-white/80 [transform:skewX(-12deg)]">
                  <span class="[transform:skewX(12deg)]">PREV</span>
                </button>
                <button type="button"
                  class="border border-white/15 bg-white/5 px-3 py-1.5 text-xs font-semibold tracking-widest text-white/80 [transform:skewX(-12deg)]">
                  <span class="[transform:skewX(12deg)]">NEXT</span>
                </button>
              </div>
            </div>

            <div class="mt-5 grid grid-cols-7 gap-2 text-xs text-white/60">
              <div v-for="d in weekDays" :key="d" class="py-2 text-center tracking-widest">
                {{ d }}
              </div>
            </div>

            <div class="mt-2 grid grid-cols-7 gap-2">
              <div v-for="cell in 35" :key="cell"
                class="aspect-square rounded-md border border-white/10 bg-white/5 p-2">
                <div class="flex items-start justify-between">
                  <span class="text-xs text-white/60">{{ (cell % 31) + 1 }}</span>
                  <span v-if="cell % 9 === 0" class="h-2 w-2 rounded-full bg-red-500" />
                </div>
                <div v-if="cell % 9 === 0" class="mt-2 text-[10px] leading-4 text-white/70">
                  Clash Day
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="lg:col-span-5">
          <div class="grid gap-4">
            <article v-for="card in cards" :key="card.title"
              class="overflow-hidden rounded-lg border border-white/10 bg-white/5">
              <div class="h-36 bg-zinc-900">
                <div
                  class="h-full w-full bg-[url('https://images.unsplash.com/photo-1521412644187-c49fa049e84d?auto=format&fit=crop&w=1200&q=60')] bg-cover bg-center opacity-40 grayscale" />
              </div>
              <div class="p-5">
                <p class="text-xs tracking-[0.25em] text-white/60">{{ card.meta }}</p>
                <h3 class="mt-2 text-lg font-black italic tracking-tight">{{ card.title }}</h3>
                <p class="mt-2 text-sm leading-7 text-white/70">
                  {{ card.desc }}
                </p>
              </div>
            </article>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
const weekDays = ['MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN']

const cards = [
  {
    meta: 'HIGHLIGHT',
    title: '爆發力衝刺｜50m 決戰（placeholder）',
    desc: '高張力賽事亮點摘要，之後可串資料（本次不做功能）。'
  },
  {
    meta: 'HIGHLIGHT',
    title: '團隊接力｜全場沸騰（placeholder）',
    desc: '以賽事轉播感呈現節奏與集體榮譽（本次僅 UI）。'
  }
]
</script>
