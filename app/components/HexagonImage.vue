<template>
    <div class="relative overflow-hidden bg-zinc-900 shadow-2xl transition-transform duration-700 hover:scale-110"
        :class="wrapperClass" :style="hexStyle">
        <img :src="src" :alt="alt" class="h-full w-full object-cover" loading="lazy" />
        <!-- 依需求：目前 Announcement 的六角圖「不用遮罩」，所以這裡不加 overlay -->
    </div>
</template>

<script setup lang="ts">
const props = withDefaults(
    defineProps<{
        src: string
        alt?: string
        wrapperClass?: string
    }>(),
    { alt: 'Event', wrapperClass: '' }
)

const { src, alt, wrapperClass } = props

const hexStyle = {
    clipPath: 'polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%)'
} as const
</script>
