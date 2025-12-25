<template>
    <footer class="border-t border-white/5 bg-black py-16 sm:py-20">
        <div class="mx-auto max-w-7xl px-4 sm:px-6">
            <div class="mb-16 grid grid-cols-1 gap-12 md:mb-20 md:grid-cols-4">
                <div class="md:col-span-2">
                    <div class="mb-8 flex items-center gap-2">
                        <div class="flex h-8 w-8 -skew-x-12 items-center justify-center bg-red-600">
                            <img src="~/assets/img/logo.jpg" alt="AOLINBIKER" class="h-8 w-8" />
                        </div>
                        <span class="text-3xl font-black italic tracking-tight text-white">傲臨彼客全民運動會</span>
                    </div>

                    <p class="mb-8 max-w-sm text-sm leading-7 text-zinc-500">
                        我們是熱血賽事的發起者，用汗水累積榮耀。加入我們，體驗真正的運動員精神。
                    </p>

                    <div class="flex flex-wrap gap-x-6 gap-y-3 text-sm">
                        <a href="https://www.instagram.com/aolinbiker.sports/"
                            class="text-zinc-400 transition-colors hover:text-white">Instagram</a>
                        <a href="https://www.threads.com/@aolinbiker.sports"
                            class="text-zinc-400 transition-colors hover:text-white">Threads</a>
                        <a href="https://www.facebook.com/ihc2819"
                            class="text-zinc-400 transition-colors hover:text-white">Facebook</a>
                    </div>
                </div>

                <div>
                    <h4 class="mb-6 text-xs font-bold uppercase tracking-widest text-white">Resources</h4>
                    <ul class="space-y-4 text-sm text-zinc-500">
                        <li><a href="#" class="transition-colors hover:text-red-500">Upcoming Events</a></li>
                        <li><a href="#" class="transition-colors hover:text-red-500">Past Results</a></li>
                        <li><a href="#" class="transition-colors hover:text-red-500">Safety Guidelines</a></li>
                        <li><a href="#" class="transition-colors hover:text-red-500">Partner with us</a></li>
                    </ul>
                </div>

                <div>
                    <h4 class="mb-6 text-xs font-bold uppercase tracking-widest text-white">Contact</h4>
                    <ul class="space-y-4 text-sm text-zinc-500">
                        <li>hello@aolinbiker.sports</li>
                        <li>Taipei, Taiwan</li>
                        <li>+886 000 000 000</li>
                    </ul>
                </div>
            </div>

            <div
                class="flex flex-col items-start justify-between gap-4 border-t border-white/5 pt-8 md:flex-row md:items-center">
                <p class="text-[10px] uppercase tracking-widest text-zinc-600">
                    &copy; 2024 AOLINBIKER SPORTS. ALL RIGHTS RESERVED.
                </p>
                <div class="flex flex-wrap gap-x-8 gap-y-2 text-[10px] uppercase tracking-widest text-zinc-600">
                    <a href="#" class="transition-colors hover:text-white">Privacy Policy</a>
                    <a href="#" class="transition-colors hover:text-white">Terms of Service</a>
                </div>
            </div>
        </div>
    </footer>
</template>

<script setup lang="ts">
// Footer：僅 UI 結構 + RWD（不做功能/資料串接）
</script>
