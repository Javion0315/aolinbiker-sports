<template>
  <section id="teams" class="relative z-30 -mt-4 overflow-hidden bg-black py-0">
    <div class="flex h-auto flex-col md:h-[80vh] md:flex-row">
      <div v-for="team in teams" :key="team.key"
        class="aolin-team-panel group relative h-[400px] cursor-pointer overflow-hidden border-r border-white/10 last:border-0 md:h-full">
        <!-- Background Image Layer -->
        <img :src="team.bgSrc" :alt="team.name"
          class="absolute inset-0 h-full w-full object-cover opacity-60  transition-all duration-700 group-hover:opacity-90" />

        <!-- Color Layer（保留隊伍色彩氛圍；hover 更熱血） -->
        <div class="absolute inset-0 opacity-20 transition-all duration-500 group-hover:opacity-75"
          :class="team.colorClass" />

        <!-- Content Overlay -->
        <div class="absolute inset-0 z-20 flex flex-col justify-end p-8 sm:p-10 md:p-12">
          <div class="translate-y-0 transition-transform duration-500 group-hover:-translate-y-4">
            <span class="mb-4 block text-4xl">{{ team.icon }}</span>
            <h3 class="text-5xl font-black italic leading-none text-white md:text-6xl">
              {{ team.name }}
            </h3>
            <p
              class="mt-3 text-sm font-bold uppercase tracking-widest text-white/60 transition-opacity duration-500 group-hover:text-white">
              {{ team.description }}
            </p>
            <div class="mt-8 h-1 w-0 bg-white transition-all duration-500 group-hover:w-full" />
          </div>
        </div>

        <!-- Texture/Noise -->
        <div
          class="pointer-events-none absolute inset-0 opacity-30 mix-blend-overlay bg-[url('https://www.transparenttextures.com/patterns/asfalt-dark.png')]" />
      </div>
    </div>

    <!-- VS Banner -->
    <div class="relative z-30 flex h-20 items-center justify-center bg-white sm:h-24">
      <div class="absolute inset-0 flex items-center overflow-hidden">
        <div
          class="aolin-vs-track flex gap-10 whitespace-nowrap text-4xl font-black italic text-black/5 sm:text-5xl md:text-6xl">
          <span v-for="i in 10" :key="i">影響綠 就是影響力！ • 大黃蜂隊 速度是我們的翅膀 • 紅勝絕對必勝 • 藍不住 超越自我極限</span>
        </div>
      </div>
      <div
        class="z-10 bg-red-600 px-10 py-2 text-4xl font-black italic text-white shadow-xl [transform:skewX(-12deg)] sm:px-12 sm:text-5xl">
        <span class="inline-block [transform:skewX(12deg)]">VS</span>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
type Team = {
  key: string
  name: string
  description: string
  icon: string
  colorClass: string
  bgSrc: string
}

// 背景圖：來自 app/assets/img/team/*
import greenBg from '../assets/img/team/green.jpg'
import redBg from '../assets/img/team/red.jpg'
import yellowBg from '../assets/img/team/yellow.jpg'
import blueBg from '../assets/img/team/blue.jpg'

const teams: Team[] = [
  {
    key: '綠隊',
    name: '影響綠',
    description: '耐力與重生',
    icon: '🏆',
    colorClass: 'bg-emerald-600',
    bgSrc: greenBg
  },
  {
    key: '紅隊',
    name: '紅勝',
    description: '力量與主宰',
    icon: '🔥',
    colorClass: 'bg-red-600',
    bgSrc: redBg
  },
  {
    key: '黃隊',
    name: '大黃蜂',
    description: '速度與敏捷',
    icon: '⚡️',
    colorClass: 'bg-yellow-500',
    bgSrc: yellowBg
  },
  {
    key: '藍隊',
    name: '藍不住',
    description: '冷靜與精準',
    icon: '❄️',
    colorClass: 'bg-blue-600',
    bgSrc: blueBg
  }
]
</script>

<style scoped>
/* 參考 framer-motion hover flex：用 flex-grow transition 模擬 */
.aolin-team-panel {
  flex: 1 1 0%;
  transition: flex-grow 500ms cubic-bezier(0.075, 0.82, 0.165, 1);
}

.aolin-team-panel:hover {
  flex-grow: 1.5;
}

@keyframes aolin-vs-marquee {
  0% {
    transform: translateX(0);
  }

  100% {
    transform: translateX(-1000px);
  }
}

.aolin-vs-track {
  animation: aolin-vs-marquee 15s linear infinite;
  will-change: transform;
}

@media (prefers-reduced-motion: reduce) {
  .aolin-team-panel {
    transition: none;
  }

  .aolin-vs-track {
    animation: none;
  }
}
</style>
