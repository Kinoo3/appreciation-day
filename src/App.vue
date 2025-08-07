<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue';

const scrollContainer = ref(null);
const currentPage = ref(0);
const numPages = 3;

onMounted(() => {
  const el = scrollContainer.value;
  if (!el) return;
  const onWheel = (e) => {
    // Always translate vertical scroll into horizontal scroll
    if (e.deltaY !== 0) {
      el.scrollLeft += e.deltaY * 50; // Increase multiplier for stronger scroll
      e.preventDefault();
    }
  };
  el.addEventListener('wheel', onWheel, { passive: false });

  const onScroll = () => {
    // Calculate current page based on scrollLeft
    const page = Math.round(el.scrollLeft / el.offsetWidth);
    currentPage.value = page;
  };
  el.addEventListener('scroll', onScroll);

  // Clean up
  onBeforeUnmount(() => {
    el.removeEventListener('wheel', onWheel);
    el.removeEventListener('scroll', onScroll);
  });
});
</script>

<template>
  <div ref="scrollContainer" class="h-screen w-screen overflow-x-scroll snap-x snap-mandatory scroll-smooth flex relative">
    <section
      class="h-screen w-screen snap-start flex items-center justify-end shrink-0 relative"
      style="background: url('/assets/zaha%20hadid.webp') left center/cover no-repeat;"
    >
      <!-- Overlay gradient for the content area on the right -->
      <div class="absolute right-0 top-0 h-full w-1/2" style="background: linear-gradient(to left, rgba(255,255,255,0.85) 60%, rgba(255,255,255,0) 100%);"></div>
      <div class="relative z-10 flex flex-col items-start justify-center h-full w-1/2 pl-16">
        <h1 class="text-5xl font-bold text-gray-900 mb-6">Welcome!</h1>
        <p class="text-lg text-gray-700 mb-8 max-w-md">This is your introduction page. Add your content here, and the left side will always show the building background, separated by a soft gradient on the content area.</p>
        <button class="px-6 py-3 bg-blue-600 text-white rounded-lg shadow">Get Started</button>
      </div>
    </section>
    <section
      class="h-screen w-screen snap-start bg-green-500 flex items-center justify-center text-white text-4xl font-bold shrink-0"
    >
      Page 2
    </section>
    <section
      class="h-screen w-screen snap-start bg-purple-500 flex items-center justify-center text-white text-4xl font-bold shrink-0"
    >
      Page 3
    </section>
  </div>
  <!-- Dots indicator at the bottom center, now fixed to viewport -->
  <div class="fixed bottom-6 left-1/2 -translate-x-1/2 flex gap-3 z-20">
    <span v-for="i in numPages" :key="i" class="w-3 h-3 rounded-full" :class="i-1 === currentPage ? 'bg-white/70' : 'bg-white/30'"></span>
  </div>
</template>

<style>
</style>