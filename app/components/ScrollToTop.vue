<template>
  <button
    v-if="isVisible"
    type="button"
    @click="scrollToTop"
    class="group fixed bottom-8 right-8 z-50 flex h-14 w-14 items-center justify-center rounded-full bg-red-600 text-white shadow-[0_0_20px_rgba(239,68,68,0.5)] transition-all duration-300 hover:bg-white hover:text-red-600 hover:shadow-[0_0_30px_rgba(239,68,68,0.7)] hover:scale-110 sm:bottom-10 sm:right-10 sm:h-16 sm:w-16"
    aria-label="回到頂部"
  >
    <svg
      class="h-6 w-6 transition-transform duration-300 group-hover:-translate-y-1 sm:h-7 sm:w-7"
      fill="none"
      stroke="currentColor"
      viewBox="0 0 24 24"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M5 10l7-7m0 0l7 7m-7-7v18"
      />
    </svg>
  </button>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const isVisible = ref(false)

const onScroll = () => {
  // 滚动超过 300px 时显示按钮
  isVisible.value = window.scrollY > 300
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

onMounted(() => {
  onScroll()
  window.addEventListener('scroll', onScroll, { passive: true })
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

