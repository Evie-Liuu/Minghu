<template>
  <div
    class="page-background content-scroller"
    @scroll="handleAppScroll"
    v-if="albums"
  >
    <header class="relative z-5 pt-25 w-full shadow-md bg-header text-rice-500">
      <div class="container mx-auto flex items-center p-4">
        <div class="w-1/3">
          <router-link to="/sdgs" class="back-home-btn">
            <span class="text">回上頁</span>
            <span class="icon">←</span>
          </router-link>
        </div>
        <div class="w-1/3"></div>
        <div class="w-1/3 text-right">
          <i class="fa-solid fa-folder-plus"></i>
        </div>
      </div>
    </header>
    <main class="p-4 px-50">
      <!-- Masonry layout -->
      <div class="columns-1 sm:columns-2 lg:columns-4 gap-4">
        <div
          v-for="album in albums"
          :key="album.id"
          class="mb-4 break-inside-avoid cursor-pointer"
          @click="openAlbumModal(album)"
        >
          <!-- 相簿封面（第一張照片） -->
          <img
            :src="getImageUrl(album.photos[0].url)"
            :alt="album.title"
            class="w-full rounded-lg shadow-md"
          />
          <!-- 相簿標題 -->
          <h3 class="mt-2 text-lg font-bold text-center">{{ album.title }}</h3>
        </div>
      </div>
    </main>

    <!-- 相簿 Modal (彈出視窗) -->
    <div
      v-if="selectedAlbum"
      class="modal-backdrop"
      @click.self="closeAlbumModal"
    >
      <div class="modal-content">
        <button class="modal-close-btn" @click="closeAlbumModal">
          &times;
        </button>

        <!-- 上排：選中圖片及其文字 -->
        <div class="modal-main-view" v-if="activeImageInModal">
          <img
            :src="getImageUrl(activeImageInModal.url)"
            alt="選中圖片"
            class="modal-active-img"
          />
          <p class="modal-active-text">{{ activeImageInModal.text }}</p>
        </div>

        <!-- 下排：瀏覽相簿全部照片 (縮圖) -->
        <div class="modal-thumbnail-rail">
          <img
            v-for="(photo, index) in selectedAlbum.photos"
            :key="index"
            :src="getImageUrl(photo.url)"
            :alt="`縮圖 ${index + 1}`"
            class="modal-thumbnail-img"
            :class="{
              active: activeImageInModal && photo.id === activeImageInModal.id,
            }"
            @click="selectImageInModal(photo)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, inject } from "vue";

const handleAppScroll = inject("handleAppScroll");

// --- 狀態管理 ---
const selectedAlbum = ref(null); // 用於儲存當前點開的相簿資料，null 表示 Modal 關閉
const activeImageInModal = ref(null); // 在 Modal 中，當前顯示的大圖

function getImageUrl(name) {
  if (!name) return "";
  return new URL(`../assets/images/${name}`, import.meta.url).href;
}

// --- 事件處理函數 ---
function openAlbumModal(album) {
  selectedAlbum.value = album;
  if (album.photos && album.photos.length > 0) {
    activeImageInModal.value = album.photos[0];
  }
}

function selectImageInModal(image) {
  activeImageInModal.value = image;
}

function closeAlbumModal() {
  selectedAlbum.value = null;
  activeImageInModal.value = null;
}

const albums = ref([
  {
    id: 1,
    title: "校園生活",
    photos: [
      { id: 101, url: "cover.png", text: "校園一景，充滿回憶的角落。" },
      { id: 102, url: "cover_sky.png", text: "從另一個角度看我們的校園。" },
    ],
  },
  {
    id: 2,
    title: "運動會",
    photos: [
      { id: 201, url: "cover_sky.png", text: "運動會開幕，藍天白雲。" },
      { id: 202, url: "cover.png", text: "緊張刺激的比賽瞬間。" },
    ],
  },
  {
    id: 3,
    title: "畢業典禮",
    photos: [
      { id: 301, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 302, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 303, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 304, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 305, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 306, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 307, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 308, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 309, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 310, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 311, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 312, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 313, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 314, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
      { id: 315, url: "Tree_1.png", text: "畢業典禮當天，陽光下的樹。" },
      { id: 316, url: "cover_sky.png", text: "驪歌響起，我們即將遠行。" },
    ],
  },
  {
    id: 4,
    title: "社團活動",
    photos: [
      { id: 401, url: "Logo.png", text: "我們社團的標誌！" },
      { id: 402, url: "cover.png", text: "社團成果展的精彩瞬間。" },
    ],
  },
  {
    id: 5,
    title: "120周年",
    photos: [
      {
        id: 501,
        url: "ChinesePaintingRing.png",
        text: "120週年校慶的特別設計。",
      },
      { id: 502, url: "cover.png", text: "校慶活動的熱鬧場面。" },
    ],
  },
]);
</script>

<style scoped>
/* Modal 樣式 */
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}
.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 10px;
  width: 90%;
  max-width: 900px;
  height: 90vh;
  max-height: 700px;
  position: relative;
  display: flex;
  flex-direction: column;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.3);
  color: #333; /* 設置文字顏色以防繼承問題 */
}
.modal-close-btn {
  position: absolute;
  top: 10px;
  right: 15px;
  background: none;
  border: none;
  font-size: 2.5rem;
  color: #666;
  cursor: pointer;
}

/* Modal 內部佈局 */
.modal-main-view {
  flex-grow: 1; /* 佔據主要空間 */
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1rem;
  overflow: hidden;
}
.modal-active-img {
  width: 100%;
  height: 75%; /* 圖片佔據預覽區大部分高度 */
  object-fit: contain; /* 保持圖片比例 */
  margin-bottom: 1rem;
}
.modal-active-text {
  font-size: 1rem;
  color: #333;
  text-align: center;
}

.modal-thumbnail-rail {
  flex-shrink: 0; /* 不讓此區域被壓縮 */
  display: flex;
  gap: 10px;
  overflow-x: auto; /* 橫向滾動 */
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}
.modal-thumbnail-img {
  width: 80px;
  height: 60px;
  object-fit: cover;
  cursor: pointer;
  border: 2px solid transparent;
  border-radius: 4px;
  transition: border-color 0.2s;
}
.modal-thumbnail-img:hover {
  border-color: #aaa;
}
.modal-thumbnail-img.active {
  border-color: #007bff; /* 高亮當前選中的縮圖 */
}
</style>
