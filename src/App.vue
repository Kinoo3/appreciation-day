<script setup>

import { onMounted, onBeforeUnmount, ref } from 'vue';
const cacarolaImg = 'assets/caçarola.jpg';
const peitoRecheadoImg = 'assets/peito-recheado.webp';
const zahaHadidImg = 'assets/zaha hadid.webp';

const scrollContainer = ref(null);
const currentPage = ref(0);
const numPages = 3;
const mealOptions = [
  {
    id: 'cacarola',
    name: 'Caçarola de carne moída',
    description: 'Carne moída temperada, assada com batatas e queijo gratinado. Acompanha salada.',
    image: cacarolaImg,
  },
  {
    id: 'peito-recheado',
    name: 'Filé de peito recheado',
    description: 'Peito de frango recheado com queijo, bacon e ervas, ao molho especial. Acompanha salada.',
    image: peitoRecheadoImg,
  },
];
const selectedMeal = ref(null);

onMounted(() => {
  const el = scrollContainer.value;
  if (!el) return;
  const onWheel = (e) => {
    if (e.deltaY !== 0) {
      el.scrollLeft += e.deltaY * 20;
      e.preventDefault();
    }
  };
  el.addEventListener('wheel', onWheel, { passive: false });

  const onScroll = () => {
    const page = Math.round(el.scrollLeft / el.offsetWidth);
    currentPage.value = page;
  };
  el.addEventListener('scroll', onScroll);

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
      :style="`background: url('${zahaHadidImg}') left center/cover no-repeat;`"
    >
      <div class="absolute right-0 top-0 h-full w-full" style="background: linear-gradient(to left, rgba(255,255,255,1) 5%, rgba(255,255,255,0.85) 15%, rgba(255,255,255,0) 60%);"></div>
      <div class="relative z-10 flex flex-col items-center justify-center h-full w-1/2 pl-16">
        <h1 class="heading mb-6">Mais um ano de muita história juntos</h1>
        <p class="text-lg text-gray-700 mb-8 max-w-md text-center">Esse é o dia de celebrarmos a sua enorme presença na minha vida</p>
      </div>
    </section>
    <section
      class="h-screen w-screen snap-start flex flex-col items-center justify-center shrink-0 relative"
      style="background: linear-gradient(to right, #fff 0%, #38a169 40%);"
    >
      <div class="text-white text-3xl font-bold mb-8 text-center">Esse ano, a sobremesa é surpresa</div>
      <div class="relative flex items-center justify-center">
        <span class="question-mark">?</span>
        <div class="sparkle-container">
          <span class="sparkle sparkle1"></span>
          <span class="sparkle sparkle2"></span>
          <span class="sparkle sparkle3"></span>
          <span class="sparkle sparkle4"></span>
        </div>
      </div>
    </section>
    <section
      class="h-screen w-screen snap-start bg-purple-500 flex items-center justify-center text-white text-4xl font-bold shrink-0"
    >
      <div class="flex flex-col items-center justify-center w-full h-full px-2">
        <div class="text-white text-2xl md:text-3xl font-bold mb-8 md:mb-12 text-center drop-shadow-lg">Selecione uma das opções de prato principal</div>
        <div class="flex flex-col md:flex-row gap-4 md:gap-16 items-center justify-center w-full">
          <div
            v-for="meal in mealOptions"
            :key="meal.id"
            class="bg-white/90 rounded-3xl shadow-2xl p-4 md:p-12 flex flex-col items-center w-full max-w-xs md:w-96 md:h-[32rem] cursor-pointer border-4 transition-all duration-200"
            :class="selectedMeal === meal.id ? 'border-yellow-400 scale-105' : 'border-transparent'"
            @click="selectedMeal = meal.id"
          >
            <img :src="meal.image" alt="" class="w-32 h-32 md:w-40 md:h-40 object-cover rounded-full mb-4 md:mb-6 shadow-lg" />
            <div class="text-lg md:text-3xl font-bold text-purple-700 mb-2 md:mb-4">{{ meal.name }}</div>
            <div class="hidden md:block text-base md:text-lg text-gray-700 mb-4 md:mb-6 text-center">{{ meal.description }}</div>
            <transition name="fade-slide">
              <div v-if="selectedMeal === meal.id" class="mt-6 md:mt-4 text-yellow-500 text-base md:text-xl font-semibold min-h-[2.5rem] flex items-center justify-center">Selecionado!</div>
            </transition>
          </div>
        </div>
        <div class="mt-8 md:mt-12 flex flex-col items-center w-full">
          <a
            v-if="selectedMeal"
            :href="`https://wa.me/5566996528862?text=Olá! Minha escolha de prato principal é: ${mealOptions.find(m => m.id === selectedMeal)?.name}`"
            target="_blank"
            class="bg-white hover:bg-gray-100 text-purple-700 font-bold py-4 px-8 rounded-full shadow-lg text-lg md:text-xl flex items-center gap-3 transition-all duration-200 border border-purple-300 w-full max-w-xs md:w-auto md:max-w-none"
          >
            <svg xmlns='http://www.w3.org/2000/svg' class='h-7 w-7' fill='none' viewBox='0 0 24 24' stroke='currentColor'><path stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M16.72 11.06a6.5 6.5 0 10-5.66 5.66l2.12-2.12a1.5 1.5 0 012.12 0l1.42 1.42a1.5 1.5 0 010 2.12l-2.12 2.12a6.5 6.5 0 005.66-5.66z' /></svg>
            Enviar escolha pelo WhatsApp
          </a>
          <span v-else class="text-white/80 text-base md:text-lg mt-4">Selecione uma opção para enviar sua escolha</span>
        </div>
      </div>
    </section>
  </div>
  <div class="fixed bottom-6 left-1/2 -translate-x-1/2 flex gap-3 z-20">
    <span v-for="i in numPages" :key="i" class="w-3 h-3 rounded-full" :class="i-1 === currentPage ? 'bg-white/70' : 'bg-white/30'"></span>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Great+Vibes&display=swap');

.fade-slide-enter-active, .fade-slide-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}
.fade-slide-enter-from, .fade-slide-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
.fade-slide-enter-to, .fade-slide-leave-from {
  opacity: 1;
  transform: translateY(0);
}


.heading {
  font-family: 'Great Vibes', cursive;
  font-size: 4rem;
  color: #2d3748;
  text-shadow: 0 2px 12px rgba(0,0,0,0.08), 0 0 24px #fff;
  letter-spacing: 2px;
  font-weight: 400;
}


.question-mark {
  font-size: 10rem;
  color: #fff;
  text-shadow: 0 0 40px #fff, 0 0 80px #ffe066;
  position: relative;
  z-index: 2;
}

.sparkle-container {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 12rem;
  height: 12rem;
  pointer-events: none;
  z-index: 1;
}

.sparkle {
  position: absolute;
  width: 1.2rem;
  height: 1.2rem;
  background: radial-gradient(circle, #fff 60%, #ffe066 100%, transparent 100%);
  border-radius: 50%;
  opacity: 0.8;
  animation: sparkle 1.5s infinite ease-in-out;
}
.sparkle1 { top: 10%; left: 60%; animation-delay: 0s; }
.sparkle2 { top: 80%; left: 30%; animation-delay: 0.5s; }
.sparkle3 { top: 40%; left: 80%; animation-delay: 1s; }
.sparkle4 { top: 70%; left: 0%; animation-delay: 1.2s; }

@keyframes sparkle {
  0%, 100% { opacity: 0.8; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.5); }
}
</style>