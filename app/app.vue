<template>
  <div class="min-h-dvh bg-black text-white overflow-x-hidden">
    <!-- 全站外框：此專案以首頁單頁架構為主，這裡只負責渲染路由 -->
    <NuxtRouteAnnouncer />
    <!-- 全站進場動畫（僅結構，預設關閉） -->
    <EntryLoader />
    <Navbar />
    <NuxtPage />
  </div>
</template>
