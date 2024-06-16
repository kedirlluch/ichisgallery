<template>
  <div>
    <nav class="bg-white border-gray-200 dark:bg-gray-900">
      <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
        <div class="hidden w-full md:block md:w-auto" id="navbar-default">
          <ul class="font-medium flex flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 rtl:space-x-reverse md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">
            <li>
              <a href="#" class="block py-2 px-3 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 dark:text-white md:dark:text-blue-500" aria-current="page">Home</a>
            </li>
            <li>
              <a href="#" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">About</a>
            </li>
            <li>
              <a href="#" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Services</a>
            </li>
            <li>
              <a href="#" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Pricing</a>
            </li>
            <li>
              <a href="#" class="block py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Contact</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-10">
      <div v-for="(row, rowIndex) in imageRows" :key="rowIndex" class="grid gap-4">
        <div v-for="(image, colIndex) in row" :key="rowIndex * 4 + colIndex">
          <img
            class="h-auto w-full rounded-lg shadow-none transition-all duration-300 cursor-pointer hover:shadow-lg hover:shadow-gray-900 hover:scale-105"
            :src="image.src"
            :alt="'Image ' + (rowIndex * 4 + colIndex + 1)"
            @click="openImageFullscreen(rowIndex * 4 + colIndex)">
        </div>
      </div>
    </div>

    <div v-if="activeImage !== null" @click="closeImageFullscreen" class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-black bg-opacity-50">
      <button @click.stop="previousImage" class="absolute left-4 text-white text-4xl">&#10094;</button>
      <img
        class="max-w-full max-h-full rounded-lg shadow-lg p-6"
        :src="images[activeImage].src"
        :alt="'Fullscreen Image ' + (activeImage + 1)">
      <button @click.stop="nextImage" class="absolute right-4 text-white text-4xl">&#10095;</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const images = ref([
  { src: 'src/assets/01.jpeg' },
  { src: 'src/assets/02.jpeg' },
  { src: 'src/assets/03.jpeg' },
  { src: 'src/assets/04.jpeg' },
  { src: 'src/assets/05.jpeg' },
  { src: 'src/assets/06.jpeg' },
  { src: 'src/assets/07.jpeg' },
  { src: 'src/assets/08.jpeg' },
  { src: 'src/assets/09.jpeg' },
  { src: 'src/assets/10.jpeg' },
  { src: 'src/assets/11.jpeg' },
  { src: 'src/assets/12.jpeg' },
  { src: 'src/assets/13.jpeg' },
  { src: 'src/assets/14.jpeg' },
  { src: 'src/assets/15.jpeg' },
]);

const activeImage = ref(null);

const openImageFullscreen = (index) => {
  activeImage.value = index;
  document.addEventListener('keydown', handleKeydown);
};

const closeImageFullscreen = () => {
  activeImage.value = null;
  document.removeEventListener('keydown', handleKeydown);
};

const nextImage = () => {
  if (activeImage.value !== null) {
    activeImage.value = (activeImage.value + 1) % images.value.length;
  }
};

const previousImage = () => {
  if (activeImage.value !== null) {
    activeImage.value = (activeImage.value - 1 + images.value.length) % images.value.length;
  }
};

const handleKeydown = (event) => {
  if (event.key === 'ArrowRight') {
    nextImage();
  } else if (event.key === 'ArrowLeft') {
    previousImage();
  } else if (event.key === 'Escape') {
    closeImageFullscreen();
  }
};

onMounted(() => {
  document.addEventListener('keydown', handleKeydown);
});

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeydown);
});

// Agrupar las imágenes en filas de 4 elementos
const imageRows = ref([]);
const groupImages = () => {
  for (let i = 0; i < images.value.length; i += 4) {
    imageRows.value.push(images.value.slice(i, i + 4));
  }
};

groupImages();
</script>

<style scoped>
/* Estilos opcionales para el fondo oscuro */
html.dark .bg-white {
  background-color: #2d3748;
}
html.dark .text-white {
  color: #ffffff;
}
</style>
