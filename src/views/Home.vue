<template>
  <main
    class="relative h-screen grid lg:grid-flow-col content-center lg:content-stretch lg:grid-rows-2 text-center font-bold bg-[url('@/assets/images/var02/Web_v2_Bcakground.webp')] bg-cover bg-center overflow-hidden lg:bg-[url('@/assets/images/var02/Backgound_v2_Hemei.webp')]"
  >
    <img
      src="@/assets/images/var02/Frame_v2_Hemei.png"
      alt="Top Frame"
      class="hidden lg:block absolute -top-3 left-0 w-full h-10 object-cover z-20 pointer-events-none"
    />
    <!-- Cover -->
    <section class="lg:row-span-2">
      <div
        :class="[
          'transition-all duration-3000 ease-out',
          isLoaded ? 'opacity-100' : 'opacity-0',
        ]"
        class="lg:w-full lg:h-full flex items-center justify-center lg:justify-start lg:pe-8"
      >
        <img
          :src="ChinesePaintingRing"
          class="hidden lg:block pointer-events-none select-none"
          alt="Hemei_cover"
        />
        <img
          :src="ChinesePaintingRing_mobile"
          class="block lg:hidden pointer-events-none select-none"
          alt="Hemei_cover"
        />
      </div>
    </section>
    <!-- School Title -->
    <section class="lg:col-span-1 lg:-ml-138 lg:mt-17 z-10">
      <div
        :class="[
          'transition-all duration-3000 ease-out',
          isLoaded ? 'opacity-100' : 'opacity-0',
        ]"
        class="flex items-right justify-center lg:justify-start lg:gap-5 w-full lg:-ms-13 lg:pt-20"
      >
        <picture>
          <img
            src="@/assets/images/var02/var02.png"
            class="hidden lg:block pointer-events-none select-none max-w-50"
            alt="Hemei_Text_H"
          />
          <img
            src="@/assets/images/var02/Text_H1_v2_Hemei.png"
            class="block lg:hidden pointer-events-none select-none max-w-54"
            alt="Hemei_Text_H"
          />
        </picture>
      </div>
    </section>
    <!-- Menu Buttons -->
    <section class="lg:col-span-1 lg:-ml-178 lg:-mt-10 z-10">
      <div
        :class="[
          'transition-all duration-3000 ease-out',
          isLoaded ? 'opacity-100' : 'opacity-0',
        ]"
        class="flex flex-col pt-8 lg:pt-0 lg:gap-5 items-center justify-center text-xl lg:text-2xl"
      >
        <router-link
          to="/about"
          class="relative"
          @mouseover="hoveredIndex = 0"
          @mouseleave="hoveredIndex = null"
        >
          <img
            :src="hoveredIndex === 0 ? paint1Selected : paint1Unselected"
            class="pointer-events-none select-none w-52 lg:w-auto"
            alt="Paint_1"
          />
          <span
            class="absolute inset-0 flex items-center justify-center text-white ps-5 tracking-widest"
            >校園探索</span
          >
        </router-link>
        <router-link
          to="/actions"
          class="relative"
          @mouseover="hoveredIndex = 1"
          @mouseleave="hoveredIndex = null"
          ><img
            :src="hoveredIndex === 1 ? paint2Selected : paint2Unselected"
            class="pointer-events-none select-none w-52 lg:w-auto"
            alt="Paint_2"
          />
          <span
            class="absolute inset-0 flex items-center justify-center text-white ps-5 tracking-widest"
            >SDGs行動</span
          ></router-link
        >
        <router-link
          to="/sdgs"
          class="relative -translate-x-1/6"
          @mouseover="hoveredIndex = 2"
          @mouseleave="hoveredIndex = null"
          ><img
            :src="hoveredIndex === 2 ? paint3Selected : paint3Unselected"
            class="pointer-events-none select-none w-52 lg:w-auto"
            alt="Paint_3"
          />
          <span
            class="absolute inset-0 flex items-center justify-center text-white ps-5 tracking-widest"
            >SDGs成果</span
          ></router-link
        >
      </div>
    </section>
    <img
      src="@/assets/images/var02/Frame_v2_Hemei.png"
      alt="Bottom Frame"
      class="hidden lg:block absolute -bottom-3 left-0 w-full h-10 object-cover rotate-180 z-20 pointer-events-none"
    />
  </main>
</template>
<script setup>
import { ref, onMounted } from "vue";
import ChinesePaintingRing from "@/assets/images/var02/ChinesePaintingRing_v2_Hemei_2.webp";
import ChinesePaintingRing_mobile from "@/assets/images/var02/Mobile_ChinesePaintingRing_v2_Hemei2.png";
import paint1Unselected from "@/assets/images/var02/Paint_1_v2_Unselected.png";
import paint1Selected from "@/assets/images/var02/Paint_1_v2_Selected.png";
import paint2Unselected from "@/assets/images/var02/Paint_2_v2_Unselected.png";
import paint2Selected from "@/assets/images/var02/Paint_2_v2_Selected.png";
import paint3Unselected from "@/assets/images/var02/Paint_3_v2_Unselected.png";
import paint3Selected from "@/assets/images/var02/Paint_3_v2_Selected.png";

const hoveredIndex = ref(null);
const isLoaded = ref(false);

onMounted(() => {
  setTimeout(() => {
    isLoaded.value = true;
  }, 100);
});
</script>
<style scoped>
</style>
