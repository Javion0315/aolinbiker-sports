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
                <a href="https://docs.google.com/forms/d/e/1FAIpQLSevLIo0wxuPcNoUNaXBoAQGcumQo-HBCB9OKiTuq5GXQOoPAg/viewform?usp=send_form"
                    class="bg-red-600 px-8 py-2 text-lg font-black italic text-white shadow-[0_0_20px_rgba(239,68,68,0.3)] transition-all hover:bg-white hover:text-red-600 [transform:skewX(-12deg)]">
                    <span class="inline-block [transform:skewX(12deg)]">JOIN NOW</span>
                </a>
            </div>

            <!-- Mobile trigger -->
            <button type="button" @click="toggleMenu" class="relative z-[60] text-white md:hidden"
                :aria-label="isMenuOpen ? 'Close menu' : 'Open menu'" :aria-expanded="isMenuOpen ? 'true' : 'false'">
                <svg v-if="!isMenuOpen" class="h-8 w-8 transition-opacity" fill="none" stroke="currentColor"
                    viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 8h16M4 16h16" />
                </svg>
                <svg v-else class="h-8 w-8 transition-opacity" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
        </div>

        <!-- Mobile Menu Overlay -->
        <Teleport to="body">
            <Transition name="mobile-menu">
                <div v-if="isMenuOpen" class="fixed inset-0 z-[100] bg-black md:hidden" @click.self="closeMenu">
                    <!-- Close Button in Menu -->
                    <button type="button" @click="closeMenu" class="absolute right-4 top-4 z-[110] text-white md:hidden"
                        aria-label="Close menu">
                        <svg class="h-8 w-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>

                    <div class="flex min-h-screen items-center justify-center px-4 py-8">
                        <nav class="flex w-full max-w-sm flex-col items-center gap-6">
                            <a v-for="item in navItems" :key="item.label" :href="item.href" @click="closeMenu"
                                class="group relative text-center text-xl font-bold tracking-widest text-white transition-colors hover:text-red-500">
                                {{ item.label }}
                                <span
                                    class="absolute -bottom-1 left-1/2 h-[2px] w-0 -translate-x-1/2 bg-red-500 transition-all duration-300 group-hover:w-full" />
                            </a>
                            <a href="https://docs.google.com/forms/d/e/1FAIpQLSevLIo0wxuPcNoUNaXBoAQGcumQo-HBCB9OKiTuq5GXQOoPAg/viewform?usp=send_form"
                                @click="closeMenu"
                                class="mt-4 bg-red-600 px-8 py-3 text-xl font-black italic text-white shadow-[0_0_20px_rgba(239,68,68,0.3)] transition-all hover:bg-white hover:text-red-600 [transform:skewX(-12deg)]">
                                <span class="inline-block [transform:skewX(12deg)]">JOIN NOW</span>
                            </a>
                        </nav>
                    </div>
                </div>
            </Transition>
        </Teleport>
    </nav>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

// uiux.md：頂部導覽列滾動超過 50px 後變更透明度與背景色
const isScrolled = ref(false)
const isMenuOpen = ref(false)

const navItems = [
    { label: 'EVENTS', href: '#announcement' },
    { label: 'TEAM', href: '#teams' },
    { label: 'SPIRIT', href: '#spirit' },
    { label: 'GALLERY', href: '#gallery' },
    { label: 'COMMUNITY', href: '#cta' }
] as const

const onScroll = () => {
    isScrolled.value = window.scrollY > 50
}

const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value
    // 當菜單打開時，鎖定 body 滾動
    if (isMenuOpen.value) {
        document.body.style.overflow = 'hidden'
        document.body.style.position = 'fixed'
        document.body.style.width = '100%'
        document.body.style.top = `-${window.scrollY}px`
    } else {
        const scrollY = document.body.style.top
        document.body.style.overflow = ''
        document.body.style.position = ''
        document.body.style.width = ''
        document.body.style.top = ''
        if (scrollY) {
            window.scrollTo(0, parseInt(scrollY || '0') * -1)
        }
    }
}

const closeMenu = () => {
    isMenuOpen.value = false
    const scrollY = document.body.style.top
    document.body.style.overflow = ''
    document.body.style.position = ''
    document.body.style.width = ''
    document.body.style.top = ''
    if (scrollY) {
        window.scrollTo(0, parseInt(scrollY || '0') * -1)
    }
}

onMounted(() => {
    onScroll()
    window.addEventListener('scroll', onScroll, { passive: true })
})

onBeforeUnmount(() => {
    window.removeEventListener('scroll', onScroll)
    // 確保清理 body overflow
    const scrollY = document.body.style.top
    document.body.style.overflow = ''
    document.body.style.position = ''
    document.body.style.width = ''
    document.body.style.top = ''
    if (scrollY) {
        window.scrollTo(0, parseInt(scrollY || '0') * -1)
    }
})
</script>

<style scoped>
/* 移動端菜單動畫 */
.mobile-menu-enter-active,
.mobile-menu-leave-active {
    transition: opacity 0.3s ease, transform 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.mobile-menu-enter-from,
.mobile-menu-leave-to {
    opacity: 0;
    transform: translateY(-20px);
}
</style>
