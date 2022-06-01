<script setup>
import { onMounted, ref } from 'vue'

const divSlides = ref([])

const slider = ref([
   { id: 1, content: '1', color: 'red' },
   { id: 2, content: '2', color: 'blue' },
   { id: 3, content: '3', color: 'orange' },
   { id: 4, content: '4', color: 'green' },
])

const activeSlide = ref(0)

const handleButton = (type) => {
   const lastIndex = slider.value.length - 1
   divSlides.value.forEach((slide) => {
      delete slide.dataset.before
   })
   divSlides.value[activeSlide.value].dataset.before = true
   switch (type) {
      case 'prev':
         if (activeSlide.value <= 0) {
            activeSlide.value = lastIndex
         } else {
            activeSlide.value -= 1
         }
         break
      case 'next':
         if (activeSlide.value >= lastIndex) {
            activeSlide.value = 0
         } else {
            activeSlide.value += 1
         }

         break
      default:
         break
   }
}
</script>

<template>
   <div class="swiper">
      <div
         :ref="(el) => (divSlides[index] = el)"
         :style="{ backgroundColor: slide.color }"
         v-for="(slide, index) in slider"
         :key="slide.id"
         class="swiper-slide"
         :class="{ 'swiper-slide-active': activeSlide === index }"
      >
         {{ slide.content }}
      </div>

      <button @click="handleButton('prev')" class="prev">Anterior</button>
      <button @click="handleButton('next')" class="next">Siguiente</button>
   </div>
</template>

<style>
.swiper {
   min-height: 100vh;
   background-color: #333;
   display: grid;
   place-items: center;
   color: white;
   position: relative;
}

.swiper-slide {
   position: absolute;
   z-index: 1;
   width: 100%;
   height: 100%;
   display: flex;
   align-items: center;
   justify-content: center;
   font-size: 3rem;
}

.swiper-slide[data-before] {
   z-index: 2;
}

.swiper-slide-active {
   animation: clipIn 1s ease forwards;
   z-index: 3;
}

.prev,
.next {
   position: absolute;
   z-index: 100;
   top: 50%;
   transform: translateY(-50%);
   left: 0;
   outline: none;
   background-color: white;
   border: 1px solid black;
   padding: 0.5rem 1rem;
   border-radius: 0.5rem;
   cursor: pointer;
}

.next {
   left: auto;
   right: 0;
}

p {
   position: absolute;
   z-index: 100;
   top: 0;
   text-align: center;
}

@keyframes clipIn {
   0% {
      clip-path: inset(0 0 100% 0);
   }

   100% {
      clip-path: inset(0 0 0 0);
   }
}
</style>
