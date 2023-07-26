<script setup lang="ts">
import { computed, ref } from 'vue'
import { useBattery, useAnimate, useBluetooth, useClipboard, useEyeDropper } from '@vueuse/core'
import DemoCard from './components/DemoCard.vue';

import colorPickerImage from './assets/color_picker.png'
import useBatteryImage from './assets/useBattery.png'
import useAnimateImage from './assets/useAnimate.png'
import HeartRate from './components/HeartRate.vue';

const el = ref(null)

const source = ref('')

const { isSupported: isSupportedUseClipboard } = useClipboard({ source })
const { isSupported: isSupportedUseEyeDropper } = useEyeDropper()



const { isSupported: batteryIsSupported } = useBattery()
const { isSupported: animateIsSupported } = useAnimate(el, {})
const { isSupported: bluetoothIsSupported } = useBluetooth()

const colorPickerIsSupported = computed(() => isSupportedUseClipboard && isSupportedUseEyeDropper)

</script>

<template>
  <main class="p-6">
    <h1 class="text-center text-2xl font-bold">@Vue.jsの勉強会はなんぼあってもいいですからね 7/27 デモ集</h1>
    <div class="flex flex-col gap-y-5 mt-8 justify-center max-w-xl m-auto">
      <DemoCard text="ColorPicker" :img="colorPickerImage" link="vuejs--vueuse-color-picker" :isSupported="colorPickerIsSupported.value" />
      <DemoCard text="useBattery" :img="useBatteryImage" link="vuejs--vueuse-battery-night-and-sun" :isSupported="batteryIsSupported" />
      <DemoCard text="useAnimate" :img="useAnimateImage" link="vuejs--vueuse-use-animation" :isSupported="animateIsSupported" />
      <div class="rounded-xl drop-shadow-md bg-emerald-600 text-white p-5" target="_blank">
        <h2 class="text-xl mb-3"># useBluetooth</h2>
        <HeartRate />
        <p class="mt-2">白ポチでBluetooth通信が始まります</p>
        <p>{{ bluetoothIsSupported ? 'お使いのブラウザでサポートされています' : 'お使いのブラウザでサポートされていません' }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
