<template>
  <div class="font-sans">

    <!-- NAVBAR -->
    <nav class="fixed w-full bg-white/90 backdrop-blur shadow z-50">
      <div class="flex justify-between items-center px-4 md:px-6 py-4 max-w-6xl mx-auto">
        <h1 class="font-bold text-blue-600">DentShine</h1>

        <button @click="open = !open" class="md:hidden text-2xl">☰</button>

        <div class="hidden md:flex space-x-6">
          <a href="#inicio">Inicio</a>
          <a href="#nosotros">Nosotros</a>
          <a href="#servicios">Servicios</a>
          <a href="#doctores">Doctores</a>
          <a href="#contacto">Contacto</a>
        </div>
      </div>

      <div v-if="open" class="md:hidden bg-white px-6 pb-4 text-center">
        <a @click="closeMenu" href="#inicio" class="block py-2">Inicio</a>
        <a @click="closeMenu" href="#nosotros" class="block py-2">Nosotros</a>
        <a @click="closeMenu" href="#servicios" class="block py-2">Servicios</a>
        <a @click="closeMenu" href="#doctores" class="block py-2">Doctores</a>
        <a @click="closeMenu" href="#contacto" class="block py-2">Contacto</a>
      </div>
    </nav>

    <!-- HERO CAROUSEL -->
    <section data-aos="fade-up" id="inicio" class="relative h-screen overflow-hidden">

      <img
        v-for="(img, index) in imagenes"
        :key="index"
        :src="img"
        class="absolute w-full h-full object-cover transition-opacity duration-700"
        :class="index === current ? 'opacity-100' : 'opacity-0'"
      />

      <div class="absolute inset-0 bg-black/50"></div>

      <div class="absolute inset-0 flex flex-col justify-center items-center text-white text-center z-10 px-4">
        <h1 class="text-3xl md:text-5xl font-bold">Clínica DentShine</h1>
        <p class="mt-3 text-sm md:text-lg">Especialistas en tu sonrisa 😁</p>

        <a :href="whatsappLink"
          class="mt-6 bg-blue-500 px-6 py-3 rounded-xl">
          Agendar cita
        </a>
      </div>

      <!-- Flechas -->
      <button @click="prev" class="absolute left-5 top-1/2 -translate-y-1/2 text-white text-3xl">❮</button>
      <button @click="next" class="absolute right-5 top-1/2 -translate-y-1/2 text-white text-3xl">❯</button>

      <!-- Indicadores -->
      <div class="absolute bottom-6 w-full flex justify-center gap-2">
        <span
          v-for="(img, index) in imagenes"
          :key="index"
          @click="current = index"
          class="w-3 h-3 rounded-full cursor-pointer"
          :class="current === index ? 'bg-white' : 'bg-gray-400'"
        ></span>
      </div>
    </section>

    <!-- NOSOTROS -->
    <section data-aos="fade-up" id="nosotros" class="px-4 py-10 md:px-12 md:py-16 text-center bg-white">
      <h2 class="text-3xl mb-4">Sobre Nosotros</h2>
      <p class="max-w-2xl mx-auto text-gray-600">
        Nos especializamos en brindar atención odontológica integral con un enfoque humano y profesional. Nuestro equipo de especialistas cuenta con amplia experiencia y está comprometido con la salud y bienestar de cada paciente.
      </p>
      <p class="max-w-2xl mx-auto mt-4 text-gray-600">
        Utilizamos tecnología moderna y tratamientos actualizados para garantizar resultados efectivos, seguros y duraderos. Nuestro objetivo es ayudarte a lograr una sonrisa sana, estética y llena de confianza.
      </p>
    </section>

    <!-- SERVICIOS -->
    <section data-aos="fade-up" id="servicios" class="px-4 py-10 md:px-12 md:py-16 bg-gray-100">
      <h2 class="text-3xl text-center mb-10">Servicios</h2>

      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
        <div v-for="s in servicios" :key="s.nombre" class="card" @click="abrirModal(s)">
          <img :src="s.imagenes[0]" class="img"/>
          <h3>{{ s.nombre }}</h3>
        </div>
      </div>
    </section>

  <!-- MODAL ULTRA PRO -->
  <transition name="overlay">
    <div
      v-if="modal"
      class="fixed inset-0 bg-black/70 backdrop-blur-md flex items-center justify-center z-50 p-4"
    >

      <transition name="modal">
        <div
          v-if="modal"
          class="relative w-full max-w-2xl bg-white/90 backdrop-blur-xl rounded-3xl shadow-[0_20px_60px_rgba(0,0,0,0.3)] overflow-hidden"
        >

          <!-- BOTÓN CERRAR FLOTANTE -->
          <button
            @click="modal = null"
            class="absolute top-4 right-4 z-10 bg-white/80 backdrop-blur-md rounded-full w-10 h-10 flex items-center justify-center shadow hover:scale-110 transition"
          >
            ✕
          </button>

          <!-- GALERÍA -->
          <div class="relative">
            <img
              :src="modal.imagenes[currentImage]"
              class="w-full h-56 object-cover transition-all duration-500"
            />

            <!-- BOTONES GALERÍA -->
            <button
              v-if="modal.imagenes.length > 1"
              @click="prevImage"
              class="absolute left-3 top-1/2 -translate-y-1/2 bg-white/70 p-2 rounded-full shadow"
            >
              ‹
            </button>

            <button
              v-if="modal.imagenes.length > 1"
              @click="nextImage"
              class="absolute right-3 top-1/2 -translate-y-1/2 bg-white/70 p-2 rounded-full shadow"
            >
              ›
            </button>
          </div>

          <!-- CONTENIDO -->
          <div class="p-6 max-h-[60vh] overflow-y-auto">

            <h3 class="text-2xl font-bold text-gray-800">
              {{ modal.nombre }}
            </h3>

            <p class="mt-3 text-gray-600 leading-relaxed">
              {{ modal.descripcion }}
            </p>

            <!-- BENEFICIOS -->
            <ul class="mt-4 space-y-2">
              <li
                v-for="(b, i) in modal.beneficios"
                :key="i"
                class="flex items-center gap-2 text-gray-700"
              >
                <span class="text-green-500">✔</span> {{ b }}
              </li>
            </ul>

            <!-- CTA -->
            <div class="mt-6 grid grid-cols-1 sm:grid-cols-2 gap-3">
              <a
                :href="whatsappLink"
                target="_blank"
                class="bg-green-500 text-white py-3 rounded-xl text-center font-semibold hover:bg-green-600 transition"
              >
                Agendar por WhatsApp
              </a>
            </div>

          </div>

        </div>
      </transition>

    </div>
  </transition>
  
    <!-- PROMOCIONES -->
    <PromoCarousel />

    <!-- DOCTORES -->
    <section data-aos="fade-up" id="doctores" class="px-4 py-10 md:px-12 md:py-16 bg-white">
      <h2 class="text-3xl text-center mb-10">Doctores</h2>

      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6 text-center">
        <div v-for="d in doctores" :key="d.nombre">
          <img :src="d.img" class="avatar"/>
          <h3>{{ d.nombre }}</h3>
          <p class="text-gray-500">{{ d.especialidad }}</p>
          <p class="text-sm text-gray-400">Cédula: {{ d.cedula }}</p>
        </div>
      </div>
    </section>

    <section class="py-16 bg-white">

      <h2 class="text-3xl font-bold text-center mb-10">
        Opiniones de nuestros pacientes
      </h2>

      <div class="max-w-6xl mx-auto grid md:grid-cols-3 gap-6">

        <!-- TESTIMONIO -->
        <div class="bg-gray-100 p-6 rounded-2xl shadow" data-aos="fade-up">
          <p class="text-gray-700 italic">
            “Excelente atención, mi sonrisa cambió completamente. Muy recomendados.”
          </p>

          <div class="mt-4 flex items-center gap-3">
            <img src="https://randomuser.me/api/portraits/women/44.jpg" class="w-10 h-10 rounded-full">
            <div>
              <p class="font-bold">María López</p>
              <p class="text-sm text-gray-500">Paciente</p>
            </div>
          </div>
        </div>

        <div class="bg-gray-100 p-6 rounded-2xl shadow" data-aos="fade-up">
          <p class="text-gray-700 italic">
            “El tratamiento fue rápido y sin dolor. Volvería sin duda.”
          </p>

          <div class="mt-4 flex items-center gap-3">
            <img src="https://randomuser.me/api/portraits/men/32.jpg" class="w-10 h-10 rounded-full">
            <div>
              <p class="font-bold">Carlos Pérez</p>
              <p class="text-sm text-gray-500">Paciente</p>
            </div>
          </div>
        </div>

        <div class="bg-gray-100 p-6 rounded-2xl shadow" data-aos="fade-up">
          <p class="text-gray-700 italic">
            “Muy profesionales, me explicaron todo el proceso. Excelente servicio.”
          </p>

          <div class="mt-4 flex items-center gap-3">
            <img src="https://randomuser.me/api/portraits/women/68.jpg" class="w-10 h-10 rounded-full">
            <div>
              <p class="font-bold">Ana Torres</p>
              <p class="text-sm text-gray-500">Paciente</p>
            </div>
          </div>
        </div>

      </div>

    </section>

    <!-- CONTACTO -->
    <section data-aos="fade-up" id="contacto" class="px-4 py-10 md:px-12 md:py-16 text-center bg-gray-100">
      <h2 class="text-3xl mb-4">Contacto</h2>

      <p>📍 Dirección: [Pon dirección]</p>
      <p>📞 Teléfono: [Pon número]</p>

      <a :href="whatsappLink"
        class="mt-4 inline-block bg-green-500 text-white px-6 py-3 rounded-xl">
        WhatsApp
      </a>
    </section>

    <!-- LOADER -->
    <div v-if="loading" class="fixed inset-0 bg-white flex flex-col items-center justify-center z-50">

      <div class="w-16 h-16 border-4 border-green-500 border-t-transparent rounded-full animate-spin"></div>

      <p class="mt-4 text-gray-600 font-semibold">
        Cargando tu mejor sonrisa...
      </p>

    </div>

    <!-- BOTÓN FLOTANTE -->
    <a :href="whatsappLink"
      class="fixed bottom-5 right-5 bg-green-500 text-white px-5 py-3 rounded-full shadow-lg">
      💬
    </a>

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import PromoCarousel from './components/PromoCarousel.vue'

const open = ref(false)
const modal = ref(null)
const loading = ref(true)

const closeMenu = () => open.value = false

const whatsappLink = "https://wa.me/521XXXXXXXXXX"


const servicios = [
  {
    nombre: "Limpieza dental",
    descripcion: "Elimina sarro y mejora tu sonrisa desde la primera sesión.",
    imagenes: [
      "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5",
      "https://images.unsplash.com/photo-1606813907291-d86efa9b94db"
    ],
    beneficios: [
      "Resultados inmediatos",
      "Sin dolor",
      "Mejora estética"
    ]
  },
  {
    nombre: "Blanqueamiento",
    descripcion: "Aclara tu sonrisa varios tonos en una sola sesión.",
    imagenes: [
      "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5",
      "https://images.unsplash.com/photo-1606813907291-d86efa9b94db"
    ],
    beneficios: [
      "Efecto rápido",
      "Seguro",
      "Sonrisa más blanca"
    ]
  },
  {
    nombre: "Ortondoncia",
    descripcion: "Corrige la posición de tus dientes para una sonrisa perfecta.",
    imagenes: [
      "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5",
      "https://images.unsplash.com/photo-1606813907291-d86efa9b94db"
    ],
    beneficios: [
      "Efecto rápido",
      "Seguro",
      "Sonrisa más blanca"
    ]
  }
]

const doctores = [
  { nombre: "Dr. Juan", especialidad: "Ortodoncia", cedula: "12345", img: "https://via.placeholder.com/200" },
  { nombre: "Dra. Ana", especialidad: "General", cedula: "67890", img: "https://via.placeholder.com/200" },
  { nombre: "Dr. Luis", especialidad: "Implantes", cedula: "54321", img: "https://via.placeholder.com/200" }
]

// Carrusel
const imagenes = [
  "https://plus.unsplash.com/premium_photo-1681966962522-546f370bc98e?q=80&w=1740&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1606811971618-4486d14f3f99?q=80&w=1548&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
  "https://images.unsplash.com/photo-1609840114035-3c981b782dfe?q=80&w=1740&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
]

const current = ref(0)
let interval = null

const next = () => current.value = (current.value + 1) % imagenes.length
const prev = () => current.value = (current.value - 1 + imagenes.length) % imagenes.length

onMounted(() => {
  interval = setInterval(next, 4000)
  setTimeout(() => {
    loading.value = false
  }, 1500) //
})

onUnmounted(() => {
  clearInterval(interval)
})

const currentImage = ref(0)

const nextImage = () => {
  currentImage.value =
    (currentImage.value + 1) % modal.value.imagenes.length
}

const prevImage = () => {
  currentImage.value =
    (currentImage.value - 1 + modal.value.imagenes.length) %
    modal.value.imagenes.length
}


const abrirModal = (servicio) => {
  modal.value = servicio
  currentImage.value = 0
}

const cerrarModal = () => {
  modal.value = null
}

</script>

<style>
html {
  scroll-behavior: smooth;
}

.card {
  @apply bg-white p-4 rounded-xl shadow hover:scale-105 transition cursor-pointer;
}

.img {
  @apply w-full h-48 object-cover rounded mb-3;
}

.avatar {
  @apply w-32 h-32 mx-auto rounded-full mb-3 object-cover shadow;
}

/* Fade fondo */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Escala modal */
.scale-enter-active {
  transition: all 0.3s ease;
}
.scale-enter-from {
  opacity: 0;
  transform: scale(0.9);
}

.scale-leave-active {
  transition: all 0.2s ease;
}
.scale-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
/* FONDO */
.overlay-enter-active,
.overlay-leave-active {
  transition: opacity 0.4s ease;
}
.overlay-enter-from,
.overlay-leave-to {
  opacity: 0;
}

/* MODAL (tipo iOS) */
.modal-enter-active {
  transition: all 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}
.modal-enter-from {
  opacity: 0;
  transform: translateY(40px) scale(0.95);
}

.modal-leave-active {
  transition: all 0.3s ease;
}
.modal-leave-to {
  opacity: 0;
  transform: translateY(20px) scale(0.95);
}
html {
  scroll-behavior: smooth;
}
</style>