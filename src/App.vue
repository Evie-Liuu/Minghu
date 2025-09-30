<template>
  <div
    v-if="isLoading && $route.path !== '/' && $route.path !== '/about'"
    class="loading-overlay"
  >
    <div class="loading-text">
      <span style="--i: 1">載</span>
      <span style="--i: 2">入</span>
      <span style="--i: 3">中</span>
      <span style="--i: 4">.</span>
      <span style="--i: 5">.</span>
      <span style="--i: 6">.</span>
    </div>
  </div>
  <nav
    class="fixed top-0 left-0 z-10 w-full p-2 text-rice-500 md:text-4xl font-bold flex flex-row justify-between items-center transition-transform duration-300 ease-in-out"
    :class="[
      $route.path !== '/' ? 'bg-header' : '',
      isHeaderVisible || isMenuOpen ? 'translate-y-0' : '-translate-y-full',
      { 'pointer-events-none': !isHeaderVisible && !isMenuOpen },
    ]"
  >
    <router-link
      to="/"
      @click="isMenuOpen = false"
      class="flex flex-row items-center gap-2"
    >
      <img
        src="@/assets/images/Logo.png"
        alt="School Logo Watercolor"
        :class="{ hidden: $route.path === '/' }"
        class="relative z-20 inset-0 w-1/11 object-center md:w-1/15"
      />
      <!-- <div
        :class="{ hidden: $route.path === '/' }"
        class="relative z-20 my-auto md:text-3xl text-2xl"
      >
        彰化市和美國民小學
      </div> -->
      <img
        src="@/assets/images/var02/Title.png"
        alt=""
        :class="{ hidden: $route.path === '/' }"
        class="relative z-20 inset-0 max-h-10 object-end md:w-auto md:max-h-15"
      />
    </router-link>

    <!-- Hamburger Button -->
    <button
      v-if="$route.path !== '/'"
      @click="isMenuOpen = !isMenuOpen"
      class="md:hidden z-20"
    >
      <i class="fa-solid fa-bars"></i>
    </button>

    <!-- Menu -->
    <div
      v-if="$route.path !== '/'"
      :class="[
        'fixed top-0 left-0 w-full flex flex-col justify-center items-center gap-8 transition-transform duration-300 ease-in-out md:relative md:h-[4rem] md:w-auto md:bg-transparent md:flex-row md:gap-3 text-xl md:text-2xl md:whitespace-nowrap',
        isMenuOpen ? 'h-screen' : 'h-0',
        isMenuOpen ? 'translate-x-0' : '-translate-x-full md:translate-x-0',
        isMenuOpen
          ? 'bg-[url(@/assets/images/var02/Web_v2_N_Bcakground.webp)] bg-cover bg-center overflow-hidden'
          : '',
      ]"
      class="text-white md:text-black md:text-shadow-none"
    >
      <div class="absolute z-[-10] inset-0 bg-black/40 md:hidden"></div>
      <router-link
        to="/"
        class="px-8 text-rice-500 text-shadow-3d"
        @click="isMenuOpen = false"
        :class="{
          'pointer-events-none text-white bg-[url(@/assets/images/Paint_3_Selected.png)] bg-cover bg-center opacity-80':
            $route.path === '/',
        }"
      >
        <div class="p-3">首頁</div>
      </router-link>
      <router-link
        to="/about"
        class="px-4 text-rice-500 text-shadow-3d"
        @click="isMenuOpen = false"
        :class="{
          'pointer-events-none text-white bg-[url(@/assets/images/Paint_3_Selected.png)] bg-cover bg-center opacity-80':
            $route.path === '/about',
          'pointer-events-none bg-gradient-to-br from-green-600 to-yellow-400 opacity-80 rounded-full':
            isMenuOpen && $route.path === '/about',
        }"
      >
        <div class="p-3">校園探索</div>
      </router-link>
      <router-link
        to="/actions"
        class="px-4 text-rice-500 text-shadow-3d"
        @click="isMenuOpen = false"
        :class="{
          'pointer-events-none text-white bg-[url(@/assets/images/Paint_3_Selected.png)] bg-cover bg-center opacity-80':
            $route.path === '/actions',
          'pointer-events-none bg-gradient-to-br from-green-600 to-yellow-400 opacity-80 rounded-full':
            isMenuOpen && $route.path === '/actions',
        }"
      >
        <div class="p-3 md:p-3">SDGs行動</div>
      </router-link>
      <router-link
        to="/sdgs"
        class="px-4 text-rice-500 text-shadow-3d"
        @click="isMenuOpen = false"
        :class="{
          'pointer-events-none text-white bg-[url(@/assets/images/Paint_3_Selected.png)] bg-cover bg-center opacity-80':
            $route.path === '/sdgs' || $route.path.includes('/story'),
          'pointer-events-none bg-gradient-to-br from-green-600 to-yellow-400 opacity-80 rounded-full':
            isMenuOpen &&
            ($route.path === '/sdgs' || $route.path.includes('/story')),
        }"
      >
        <div class="p-3">SDGs成果</div>
      </router-link>
    </div>
  </nav>
  <router-view />
</template>
<script setup>
import { ref, onMounted, provide, onBeforeUnmount } from "vue";

const isLoading = ref(true);
const isMenuOpen = ref(false);
const isHeaderVisible = ref(true);
const lastScrollY = ref(0);

const handleScroll = (event) => {
  // Get scroll position from either the event target or the window
  const currentScrollY =
    event.target.scrollTop === undefined
      ? window.scrollY
      : event.target.scrollTop;

  // Always show header when at the top or if the menu is open
  if (currentScrollY < 100 || isMenuOpen.value) {
    isHeaderVisible.value = true;
  } else if (currentScrollY > lastScrollY.value) {
    // Scrolling down
    isHeaderVisible.value = false;
  } else {
    // Scrolling up
    isHeaderVisible.value = true;
  }
  lastScrollY.value = currentScrollY < 0 ? 0 : currentScrollY;
};

provide("handleAppScroll", handleScroll);

onMounted(() => {
  window.addEventListener("scroll", handleScroll); // Fallback for pages that scroll the window
  setTimeout(() => {
    isLoading.value = false;
  }, 1000);
});

onBeforeUnmount(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>
<style scoped>
</style>