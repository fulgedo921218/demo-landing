<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const promociones = [
  {
    img: "https://images.unsplash.com/photo-1606813907291-d86efa9b94db",
    texto: "Limpieza + Blanqueamiento desde $499",
  },
  {
    img: "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5",
    texto: "Valoración GRATIS este mes",
  },
  {
    img: "https://images.unsplash.com/photo-1609840112855-9e4c3b2f3b5d",
    texto: "Ortodoncia sin dolor",
  }
]

const current = ref(0)
let interval = null

const next = () => {
  current.value = (current.value + 1) % promociones.length
}

const prev = () => {
  current.value =
    (current.value - 1 + promociones.length) % promociones.length
}

/* AUTO PLAY INTELIGENTE */
const start = () => {
  interval = setInterval(next, 4000)
}

const stop = () => {
  clearInterval(interval)
}

onMounted(start)
onUnmounted(stop)

/* SWIPE */
let startX = 0

const touchStart = (e) => {
  startX = e.touches[0].clientX
  stop()
}

const touchEnd = (e) => {
  const endX = e.changedTouches[0].clientX

  if (startX - endX > 50) next()
  if (endX - startX > 50) prev()

  start()
}
</script>

<template>
  <section class="py-16 bg-gray-100">

    <h2 class="text-3xl font-bold text-center mb-10">
      Promociones
    </h2>

    <div
      class="relative max-w-5xl mx-auto overflow-hidden rounded-3xl shadow-2xl"
      @mouseenter="stop"
      @mouseleave="start"
      @touchstart="touchStart"
      @touchend="touchEnd"
    >

      <!-- SLIDES -->
      <div
        class="flex transition-transform duration-700 ease-[cubic-bezier(0.22,1,0.36,1)]"
        :style="{ transform: `translateX(-${current * 100}%)` }"
      >
        <div
          v-for="(promo, i) in promociones"
          :key="i"
          class="min-w-full relative"
        >
          <img
            :src="promo.img"
            class="w-full h-[300px] md:h-[400px] object-cover"
          />

          <!-- OVERLAY -->
          <div class="absolute inset-0 bg-black/40 flex flex-col justify-center items-center text-center text-white px-6">

            <h3 class="text-2xl md:text-4xl font-bold mb-4 animate-fade">
              {{ promo.texto }}
            </h3>

            <a
              href="#contacto"
              class="bg-green-500 px-6 py-3 rounded-xl font-semibold hover:scale-105 transition"
            >
              Agendar ahora
            </a>

          </div>
        </div>
      </div>

      <!-- BOTONES -->
      <button
        @click="prev"
        class="absolute left-4 top-1/2 -translate-y-1/2 bg-white/80 p-3 rounded-full shadow hover:scale-110 transition"
      >
        ‹
      </button>

      <button
        @click="next"
        class="absolute right-4 top-1/2 -translate-y-1/2 bg-white/80 p-3 rounded-full shadow hover:scale-110 transition"
      >
        ›
      </button>

      <!-- INDICADORES -->
      <div class="absolute bottom-4 left-1/2 -translate-x-1/2 flex gap-2">
        <div
          v-for="(p, i) in promociones"
          :key="i"
          @click="current = i"
          class="h-2 rounded-full transition-all duration-300 cursor-pointer"
          :class="i === current ? 'w-6 bg-green-500' : 'w-2 bg-white/60'"
        ></div>
      </div>

    </div>
  </section>
</template>

<style>
/* animación texto */
.animate-fade {
  animation: fadeUp 0.8s ease;
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>