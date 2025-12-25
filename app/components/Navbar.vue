<template>
    <nav class="fixed left-0 top-0 z-40 w-full transition-all duration-300"
        :class="isScrolled ? 'bg-black/90 py-4 shadow-2xl backdrop-blur-md' : 'bg-transparent py-6 sm:py-8'">
        <div class="mx-auto flex max-w-7xl items-center justify-between px-4 sm:px-6">
            <!-- TODO(animation): 進場淡入/左滑（可用 GSAP 或 CSS），此處只保留結構 -->
            <a href="#hero" class="flex items-center gap-2">
                <div class="flex h-8 w-8 -skew-x-12 rounded-full items-center justify-center bg-red-600">
                    <img src="~/assets/img/logo.jpg" alt="AOLINBIKER" class="h-8 w-8 rounded-full" />
                </div>
                <span class="text-2xl font-black italic tracking-tight text-white">傲臨彼客全民運動會</span>
            </a>

            <div class="hidden items-center gap-10 md:flex">
                <a v-for="item in navItems" :key="item.label" :href="item.href"
                    class="group relative text-xs font-bold tracking-widest text-white transition-colors hover:text-red-500">
                    {{ item.label }}
                    <span
                        class="absolute -bottom-1 left-0 h-[2px] w-0 bg-red-500 transition-all duration-300 group-hover:w-full" />
                </a>

                <!-- TODO(animation): hover/tap 微互動（scale/skew），此處只用 Tailwind 做視覺 -->
                <a href="#cta"
                    class="bg-red-600 px-8 py-2 text-lg font-black italic text-white shadow-[0_0_20px_rgba(239,68,68,0.3)] transition-all hover:bg-white hover:text-red-600 [transform:skewX(-12deg)]">
                    <span class="inline-block [transform:skewX(12deg)]">JOIN NOW</span>
                </a>
            </div>

            <!-- Mobile trigger（placeholder，不做展開功能） -->
            <button type="button" class="text-white md:hidden" aria-label="Open menu (placeholder)">
                <svg class="h-8 w-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 8h16M4 16h16" />
                </svg>
            </button>
        </div>
    </nav>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

// uiux.md：頂部導覽列滾動超過 50px 後變更透明度與背景色
const isScrolled = ref(false)

const navItems = [
    { label: 'EVENTS', href: '#announcement' },
    { label: 'SPIRIT', href: '#spirit' },
    { label: 'GALLERY', href: '#gallery' },
    { label: 'COMMUNITY', href: '#cta' }
] as const

const onScroll = () => {
    isScrolled.value = window.scrollY > 50
}

onMounted(() => {
    onScroll()
    window.addEventListener('scroll', onScroll, { passive: true })
})

onBeforeUnmount(() => {
    window.removeEventListener('scroll', onScroll)
})
</script>
