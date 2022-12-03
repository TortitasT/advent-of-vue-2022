<script setup>
import CountdownHeader from '@/components/CountdownHeader.vue'
import CountdownSegment from './components/CountdownSegment.vue'
import { useNow } from '@vueuse/core'
import { ref, computed, watch } from 'vue'

const now = useNow()
const christmas = new Date('12/25/2022 00:00:00')

const days = computed(() => {
  const diff = christmas.getTime() - now.value.getTime()
  return Math.floor(diff / (1000 * 60 * 60 * 24))
})
const hours = computed(() => {
  const diff = christmas.getTime() - now.value.getTime()
  return Math.floor((diff / (1000 * 60 * 60)) % 24)
})
const minutes = computed(() => {
  const diff = christmas.getTime() - now.value.getTime()
  return Math.floor((diff / 1000 / 60) % 60)
})
const seconds = computed(() => {
  const diff = christmas.getTime() - now.value.getTime()
  return Math.floor((diff / 1000) % 60)
})

const segments = [
  { label: 'Days', number: days },
  { label: 'Hours', number: hours },
  { label: 'Minutes', number: minutes },
  { label: 'Seconds', number: seconds },
]
</script>
<template>
  <div class="w-full h-full flex justify-center items-center p-10">
    <div>
      <div class="shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px]">
        <CountdownHeader />
        <main class="flex justify-center">
          <CountdownSegment v-for="segment in segments" :label="segment.label" :number="segment.number" />
        </main>
      </div>
      <h4 class="mt-10 text-gray-400 text-center text-sm">
        This challenge brought to you by <a href="https://vueschool.io/" class="underline">Vue School</a>
      </h4>
    </div>
  </div>
</template>

<style>
div {
  display: block;
}

body {
  @apply bg-gray-100;
}
</style>
