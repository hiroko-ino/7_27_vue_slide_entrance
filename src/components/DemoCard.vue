<script setup lang="ts">

const props = defineProps({
  img: {
    type: String
  },
  text: {
    type: String
  },
  isSupported: {
    type: Boolean
  },
  link: {
    type: String,
  }
})

</script>

<template>
  <div class="rounded-xl drop-shadow-md bg-emerald-600 text-white p-5">
    <h2 class="text-xl mb-3"># {{ props.text }}</h2>
    <img class="object-cover w-full h-40 rounded-2xl" :src="img">
    <p class="mt-2"><a class="underline" :href="`https://hiroko-ino.github.io/${link}/`" target="_blank">Demo</a>　<a class="underline" :href="`https://github.com/hiroko-ino/${link}`" target="_blank">GitHub</a></p>
    <p class="mt-2">{{ isSupported ? 'お使いのブラウザでサポートされています' : 'お使いのブラウザでサポートされていません' }}</p>
    </div>
</template>
