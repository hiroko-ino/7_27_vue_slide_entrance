<script setup lang="ts">
import { pausableWatch, useBluetooth } from '@vueuse/core'
import { ref } from 'vue';
import HeartRateShape from '../assets/heart_shape.svg'

// 心拍数
const heartRateValue = ref<undefined | number>(0)

// 心拍数を得たかどうか
const isGettingHeartRate = ref(false)

// 心拍数を表示するかどうか
const shownHeartRate = ref(false)

const {
  isConnected,
  requestDevice,
  device,
  server,
} = useBluetooth({
  acceptAllDevices: true,
  optionalServices: [
    'heart_rate'
  ],
})

/**
 * Bluetoothを通じて心拍数を得る
 */
const getHeartRate = async () => {
  isGettingHeartRate.value = true

  if (!server.value) return

  const heartRateService = await server.value.getPrimaryService('heart_rate')

  const heartRateCharacteristic = await heartRateService.getCharacteristic(
    'heart_rate_measurement',
  )

  heartRateCharacteristic.startNotifications();

  // @ts-ignore
  heartRateCharacteristic.addEventListener('characteristicvaluechanged', (event) => {
    // @ts-ignore
    const value = event.target.value
    if (value.getUint8(0) & 0x01) {
      heartRateValue.value = value.getUint16(1, /*littleEndian=*/true)
    } else {
      heartRateValue.value = value.getUint8(1)
    }
    
  })
}

const { stop } = pausableWatch(isConnected, (newIsConnected) => {
  if (!newIsConnected || !server.value || isGettingHeartRate.value) return
  getHeartRate()
  stop()
})

</script>

<template>
  <div class="w-36">
    <div class="bg-slate-900 text-center p-4 rounded-xl text-white">
        <span v-if="isConnected">heartRate<br></span>
        <span v-if="isConnected" class="text-white text-2xl">{{ heartRateValue }}</span>
        <button v-if="!isConnected" @click="requestDevice">●</button>
    </div>
  </div>
</template>

<style>
@keyframes heart_scale {
  0% {
    transform: scale(0.9)
  }
  70% {
    transform: scale(1.03)
  }
  100% {
    transform: scale(0.9)
  }
}

.heart {
  animation: heart_scale 3s infinite;
}
</style>