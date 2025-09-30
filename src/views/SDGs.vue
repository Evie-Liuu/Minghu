<template>
  <div class="page-background">
    <div class="content-scroller" @scroll="handleAppScroll">
      <header class="pt-25 w-full shadow-md z-10 bg-header text-rice-500">
        <div class="container mx-auto flex items-center p-4">
          <div class="w-1/3">
            <router-link to="/" class="back-home-btn">
              <span class="text">回首頁</span>
              <span class="icon">←</span>
            </router-link>
          </div>
          <div class="w-1/3 text-center">
            <h1 class="text-2xl md:text-3xl font-bold">成果展示</h1>
          </div>
          <div class="w-1/3"></div>
        </div>
      </header>
      <main
        class="grid grid-cols-1 md:grid-cols-3 gap-5 p-4 md:px-50 pt-10 text-center text-2xl md:text-3xl md:h-3/4"
      >
        <div
          class="group col-span-1 md:col-span-1 h-48 md:h-auto text-shadow-lg/30 border p-4 bg-[url(@/assets/images/cover_120.jpg)] bg-cover bg-top overflow-hidden flex items-center justify-center animate-fade-in-left animate-delay-100 text-white text-shadow-3d border-black"
        >
          <!-- <div class="absolute inset-0 bg-black/40"></div> -->
          數據呈現
        </div>
        <div
          class="group col-span-1 md:col-span-1 h-48 md:h-auto text-shadow-lg/30 border p-4 bg-[url(@/assets/images/cover_sky.png)] bg-cover bg-top overflow-hidden flex items-center justify-center animate-fade-in-left animate-delay-200 text-white text-shadow-3d border-black"
        >
          <!-- <div class="absolute inset-0 bg-black/40"></div> -->
          下載資源
        </div>

        <router-link
          class="group col-span-1 md:col-span-1 h-48 md:h-auto text-shadow-lg/30 border p-4 bg-[url(@/assets/images/cover_playground.png)] bg-cover bg-center overflow-hidden flex items-center justify-center animate-fade-in-left animate-delay-300 hover:shadow-[10px_5px_10px_-3px_rgba(0,0,0,0.8)] hover:font-bold text-white text-shadow-3d border-black"
          to="/story"
        >
          <!-- <div
        class="absolute inset-0 bg-black/40 lg:group-hover:hidden hidden lg:block"
      ></div> -->
          故事牆</router-link
        >
      </main>
    </div>
  </div>
</template>
<script setup>
import { inject } from "vue";

const handleAppScroll = inject("handleAppScroll");
</script>
<style scoped>
</style>