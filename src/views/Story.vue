<template>
  <div class="actions-wrapper page-background">
    <!-- Left decorative clouds -->
    <div class="cloud-container-left">
      <img
        v-for="cloud in leftClouds"
        :key="cloud.id"
        :src="cloud.src"
        :style="cloud.style"
        alt="Decorative Cloud"
      />
    </div>
    <!-- Right decorative clouds -->
    <div class="cloud-container-right">
      <img
        v-for="cloud in rightClouds"
        :key="cloud.id"
        :src="cloud.src"
        :style="cloud.style"
        alt="Decorative Cloud"
      />
    </div>

    <div class="content-scroller" @scroll="handleAppScroll">
      <header
        class="relative z-5 pt-25 w-full shadow-md bg-header text-rice-500"
      >
        <div class="container mx-auto flex items-center p-4">
          <div class="w-1/3">
            <router-link to="/sdgs" class="back-home-btn">
              <span class="text">回上頁</span>
              <span class="icon">←</span>
            </router-link>
          </div>
          <div class="w-1/3 text-center">
            <h1 class="text-2xl md:text-3xl font-bold">故事牆</h1>
          </div>
          <div class="w-1/3"></div>
        </div>
      </header>
      <main
        class="p-10 flex flex-col justify-center items-center gap-8 md:mx-auto"
      >
        <HeaderFilter
          @update:filters="handleFilterUpdate"
          class="flex flex-wrap justify-center items-center gap-4"
        ></HeaderFilter>
        <section
          class="grid grid-cols-1 content-center md:grid-cols-3 gap-5 p-4 md:p-0 md:gap-5"
        >
          <div
            class="animate-fade-in-up cursor-pointer md:mx-auto min-h-80 bg-white rounded-xl shadow-md overflow-hidden flex flex-col gap-5 hover:scale-105"
            v-for="info in paginatedInfos"
            :key="info.id"
            @click="goToStory(info.id)"
          >
            <img
              src="@/assets/images/cover.png"
              alt="Card Image"
              class="h-45 object-cover"
            />
            <div class="p-4 flex flex-col justify-center">
              <h2 class="text-xl font-bold mb-2">{{ info.title }}</h2>
              <p class="text-gray-600 text-sm">{{ info.intro }}</p>
              <p class="text-gray-600 text-sm text-right pe-3">更多</p>
            </div>
          </div>
        </section>
        <div
          v-if="allFilteredInfos.length === 0"
          class="text-center text-gray-500 mt-8"
        >
          <p>找不到符合條件的故事。</p>
        </div>
        <PageLabel
          class="flex justify-center items-center gap-4 mt-8"
          :totalItems="allFilteredInfos.length"
          :itemsPerPage="itemsPerPage"
          v-model="currentPage"
        ></PageLabel>
      </main>
    </div>
  </div>
</template>
<script setup>
import { ref, computed, onMounted, inject } from "vue";
import { useRouter } from "vue-router";
import infosData from "@/data/Hemei_story.json";
import typeTags from "@/data/SDGs_goal.json";
import HeaderFilter from "@/components/HeaderFilter.vue";
import PageLabel from "@/components/PageLabel.vue";

// Import tree images
import cloud1 from "@/assets/images/Tree_1.png";

// const cloudImages = [cloud1, ];
const leftClouds = ref([]);
const rightClouds = ref([]);

const generateClouds = (count) => {
  const trees = [];
  for (let i = 0; i < count; i++) {
    // const randomTreeIndex = Math.floor(Math.random() * treeImages.length);
    trees.push({
      id: `tree-${i}`,
      // src: treeImages[randomTreeIndex],
      src: cloud1,
      style: {
        top: `${i * 150 + Math.random() * 30}px`, // Overlap by setting step less than image height
        transform: `rotate(${(Math.random() - 0.5) * 15}deg) scaleX(${
          Math.random() > 0.5 ? 1 : -1
        })`,
        left: `${(Math.random() - 0.5) * 60}px`, // Horizontal jitter
        zIndex: i,
      },
    });
  }
  return trees;
};

onMounted(() => {
  // leftClouds.value = generateClouds(10);
  // rightClouds.value = generateClouds(10);
});

const handleAppScroll = inject("handleAppScroll");

const allInfos = ref(infosData);
const filters = ref({
  sdgs: [],
  time: "all",
  startDate: "",
  endDate: "",
});
const currentPage = ref(1);
const itemsPerPage = 6;

const handleFilterUpdate = (newFilters) => {
  filters.value = newFilters;
  currentPage.value = 1; // Reset page when filters change
};

const allFilteredInfos = computed(() => {
  let result = allInfos.value;

  // Filter by SDGs
  if (filters.value.sdgs.length > 0) {
    result = result.filter(
      (info) =>
        info.types &&
        info.types.some((type) => filters.value.sdgs.includes(type))
    );
  }

  // Filter by time
  if (filters.value.time !== "all") {
    const now = new Date();
    result = result.filter((info) => {
      if (!info.time || String(info.time).trim() === "") return false;
      const infoDate = new Date(Number(info.time) * 1000);

      if (filters.value.time === "day") {
        const yesterday = new Date();
        yesterday.setDate(now.getDate() - 1);
        return infoDate >= yesterday;
      } else if (filters.value.time === "week") {
        const lastWeek = new Date();
        lastWeek.setDate(now.getDate() - 7);
        return infoDate >= lastWeek;
      } else if (
        filters.value.time === "custom" &&
        filters.value.startDate &&
        filters.value.endDate
      ) {
        const start = new Date(filters.value.startDate);
        start.setHours(0, 0, 0, 0);
        const end = new Date(filters.value.endDate);
        end.setHours(23, 59, 59, 999);
        return infoDate >= start && infoDate <= end;
      }
      return true; // Should not happen if time is not 'all', but as a fallback
    });
  }

  return result;
});

const paginatedInfos = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  return allFilteredInfos.value.slice(startIndex, endIndex);
});

const router = useRouter();
const goToStory = (id) => {
  router.push({ name: "story-detail", params: { id } });
};
</script>
<style scoped>
.actions-wrapper {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  /* Keep clouds contained */
}

.cloud-container-left,
.cloud-container-right {
  position: absolute;
  top: 140px;
  bottom: 0;
  width: 220px; /* Adjust width to contain clouds */
  pointer-events: none;
  z-index: 0;
}

.cloud-container-left {
  left: -55px;
}

.cloud-container-right {
  right: -55px;
}

.cloud-container-left img,
.cloud-container-right img {
  position: absolute;
  width: 200px;
  height: auto;
}

/* RWD: 在小螢幕上隱藏裝飾圖片 */
@media (max-width: 1480px) {
  .cloud-container-left,
  .cloud-container-right {
    display: none;
  }
}
</style>