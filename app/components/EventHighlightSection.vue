<template>
  <section id="highlights" class="relative overflow-hidden bg-black py-32">
    <!-- Background Graphic -->
    <div class="pointer-events-none absolute top-0 left-0 h-full w-full select-none opacity-[0.02]">
      <h2 class="absolute -left-20 -top-20 font-black italic leading-none text-[50vw] text-white">2026</h2>
    </div>

    <div class="relative z-10 mx-auto max-w-7xl px-6">
      <!-- Header Section -->
      <div class="mb-12 flex flex-col gap-6 sm:mb-16 md:mb-20 md:flex-row md:items-end md:justify-between md:gap-8">
        <div class="min-w-0 flex-1">
          <div class="mb-3 flex items-center gap-3 sm:mb-4 sm:gap-4">
            <div class="h-[2px] w-8 bg-red-600 sm:w-12" />
            <span
              class="font-black uppercase tracking-[0.3em] text-[10px] text-red-500 sm:text-xs sm:tracking-[0.4em]">Annual
              Roadmap</span>
          </div>
          <h2 class="font-black italic leading-[0.75] tracking-tighter text-5xl sm:text-6xl md:text-7xl lg:text-9xl">
            <span class="block">2026</span>
            <span class="block">
              <span class="aolin-text-stroke text-white/20">ANNUAL</span><br class="hidden sm:block" />
              <span class="sm:ml-0">PREVIEW.</span>
            </span>
          </h2>
        </div>
        <div class="hidden text-right md:block md:flex-shrink-0">
          <p class="max-w-[350px] text-sm font-bold uppercase tracking-widest text-zinc-500">
            提前佈局你的賽季，在每一個轉折點證明實力。
          </p>
        </div>
      </div>

      <!-- 12 Months Grid -->
      <div class="grid grid-cols-2 gap-3 sm:gap-4 md:grid-cols-3 md:gap-6 lg:grid-cols-4">
        <div v-for="(item, idx) in yearCalendar" :key="item.num"
          class="group relative aspect-square cursor-default overflow-hidden border-2 bg-zinc-900/50 p-4 transition-all duration-500 hover:scale-[1.02] hover:bg-zinc-800 sm:p-5 md:p-6 lg:p-8"
          :class="[
            item.borderColor,
            item.isActive ? 'opacity-100 shadow-[0_10px_30px_rgba(0,0,0,0.5)]' : 'opacity-50 grayscale'
          ]">
          <!-- Background Image for active months -->
          <img v-if="item.imageUrl" :src="item.imageUrl" :alt="item.monthName"
            class="absolute inset-0 h-full w-full scale-110 object-cover opacity-40 grayscale transition-all duration-700 group-hover:scale-100 group-hover:opacity-60 group-hover:grayscale-0"
            loading="lazy" />
          <!-- Gradient Overlay -->
          <div v-if="item.imageUrl"
            class="absolute inset-0 z-0 bg-gradient-to-t from-black via-black/40 to-transparent" />

          <!-- Corner Accent (右上角裝飾邊框) -->
          <div class="absolute top-0 right-0 z-10 h-6 w-6 border-r-4 border-t-4 opacity-40 sm:h-8 sm:w-8"
            :class="item.borderColor" />

          <div class="relative z-10 flex h-full flex-col justify-between">
            <!-- 上半部：月份數字與名稱 -->
            <div>
              <!-- 大號月份數字（hover 時變亮） -->
              <span
                class="mb-1 block font-black italic leading-none text-white transition-all duration-500 sm:text-5xl md:text-6xl lg:text-8xl text-4xl"
                :class="item.isActive ? 'opacity-30 group-hover:opacity-100' : 'opacity-10 text-gray-200'">
                {{ item.num }}
              </span>
              <!-- 月份名稱 -->
              <span class="text-[9px] font-black uppercase tracking-[0.4em] sm:text-[10px] sm:tracking-[0.5em]"
                :class="item.isActive ? 'text-zinc-100' : 'text-zinc-600'">
                {{ item.monthName }}
              </span>
            </div>

            <!-- 下半部：活動內容 -->
            <div v-if="item.isActive" class="mt-2 sm:mt-4">
              <div class="mb-1 flex flex-wrap items-center gap-1.5 sm:mb-2 sm:gap-2">
                <!-- 圖標：年度季賽用閃電，兒童賽事用盾牌（帶背景圓形） -->
                <div
                  class="flex h-5 w-5 flex-shrink-0 items-center justify-center rounded-full bg-white/10 backdrop-blur-sm sm:h-6 sm:w-6">
                  <svg v-if="item.type === 'SEASON'" class="h-3 w-3 text-white sm:h-4 sm:w-4" fill="none"
                    stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M13 10V3L4 14h7v7l9-11h-7z" />
                  </svg>
                  <svg v-else-if="item.type === 'KIDS'" class="h-3 w-3 text-white sm:h-4 sm:w-4" fill="none"
                    stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z" />
                  </svg>
                </div>
                <h4 class="text-sm font-bold italic text-white drop-shadow-md sm:text-base md:text-lg lg:text-xl">
                  {{ item.title }}
                </h4>
              </div>
            </div>
            <!-- 無活動狀態（提高亮度） -->
            <div v-else class="mt-2 sm:mt-4">
              <p class="text-sm font-bold uppercase tracking-widest text-gray-200">休賽整備期</p>
            </div>
          </div>

          <!-- Decorative Indicator Line (底部條形指示器，hover 時展開) -->
          <div class="absolute bottom-0 left-0 z-20 h-1 w-0 transition-all duration-700 group-hover:w-full"
            :class="item.borderColor.replace('border-', 'bg-')" />
        </div>
      </div>

      <!-- Legend (圖例) -->
      <div
        class="mt-12 flex flex-wrap justify-center gap-4 border-t border-white/5 pt-8 sm:mt-16 sm:gap-6 sm:pt-12 md:gap-8">
        <div class="flex items-center gap-2 sm:gap-3">
          <div class="h-3 w-3 rounded-full bg-red-600 shadow-[0_0_10px_rgba(239,68,68,0.5)] sm:h-4 sm:w-4" />
          <span class="text-[10px] font-bold uppercase tracking-wider text-zinc-400 sm:text-xs sm:tracking-widest">年度季賽
            (春/秋/冬)</span>
        </div>
        <div class="flex items-center gap-2 sm:gap-3">
          <div class="h-3 w-3 rounded-full bg-lime-400 shadow-[0_0_10px_rgba(163,230,53,0.5)] sm:h-4 sm:w-4" />
          <span
            class="text-[10px] font-bold uppercase tracking-wider text-zinc-400 sm:text-xs sm:tracking-widest">兒童專屬賽事</span>
        </div>
        <div class="flex items-center gap-2 sm:gap-3">
          <div class="h-3 w-3 rounded-full bg-zinc-800 opacity-30 sm:h-4 sm:w-4" />
          <span
            class="text-[10px] font-bold uppercase tracking-wider text-zinc-400 sm:text-xs sm:tracking-widest">整備與休賽期</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
// 導入圖片
import redImg from '../assets/img/calendar/red.jpg'
import red2Img from '../assets/img/calendar/red2.jpg'
import greenImg from '../assets/img/calendar/green.jpg'
import green2Img from '../assets/img/calendar/green2.jpg'
import yellowImg from '../assets/img/calendar/yellow.jpg'
import yellow2Img from '../assets/img/calendar/yellow2.JPG'
import purpleImg from '../assets/img/calendar/group.jpg' // 使用 group.jpg 作為紫色
import blueImg from '../assets/img/calendar/blue.jpg' // 用於10月
import orangeImg from '../assets/img/calendar/yellow2.JPG' // 橙色使用 yellow2

import child1Img from '../assets/img/calendar/child/child1.JPG'
import child2Img from '../assets/img/calendar/child/child2.JPG'
import child3Img from '../assets/img/calendar/child/child3.JPG'

// 年曆資料結構
type MonthData = {
  num: string // 月份數字（01-12）
  monthName: string // 月份名稱（中文）
  title?: string // 活動標題
  desc?: string // 活動描述
  borderColor: string // Tailwind 邊框顏色類
  isActive: boolean // 是否有活動
  type?: 'SEASON' | 'KIDS' | 'NONE' // 活動類型
  imageUrl?: string // 背景圖片 URL
}

const yearCalendar: MonthData[] = [
  {
    num: '01',
    monthName: '一月',
    borderColor: 'border-zinc-800',
    isActive: false,
    type: 'NONE'
  },
  {
    num: '02',
    monthName: '二月',
    borderColor: 'border-zinc-800',
    isActive: false,
    type: 'NONE'
  },
  {
    num: '03',
    monthName: '三月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-red-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: redImg
  },
  {
    num: '04',
    monthName: '四月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-red-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: red2Img
  },
  {
    num: '05',
    monthName: '五月',
    title: '兒童傲臨彼客',
    desc: '5月、11月',
    borderColor: 'border-lime-400',
    isActive: true,
    type: 'KIDS',
    imageUrl: child1Img
  },
  {
    num: '06',
    monthName: '六月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-green-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: greenImg
  },
  {
    num: '07',
    monthName: '七月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-green-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: green2Img
  },
  {
    num: '08',
    monthName: '八月',
    borderColor: 'border-zinc-800',
    isActive: false,
    type: 'NONE'
  },
  {
    num: '09',
    monthName: '九月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-purple-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: purpleImg
  },
  {
    num: '10',
    monthName: '十月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-purple-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: blueImg
  },
  {
    num: '11',
    monthName: '十一月',
    title: '兒童傲臨彼客',
    desc: '5月、11月',
    borderColor: 'border-lime-400',
    isActive: true,
    type: 'KIDS',
    imageUrl: child2Img
  },
  {
    num: '12',
    monthName: '十二月',
    title: '年度季賽',
    desc: '3-4月、6-7月、9-10月、12月',
    borderColor: 'border-orange-600',
    isActive: true,
    type: 'SEASON',
    imageUrl: orangeImg
  }
]
</script>
