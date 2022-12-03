<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <p class="text-3xl p-5">{{ joke }}</p>
    <button @click="tellMe" name="Tell Me!"
      class="border border-slate-300 rounded-md p-3 focus:outline-none focus:ring focus:ring-sky-300">Tell
      Me!</button>
    <button @click="loadJoke" name="Another"
      class="border border-slate-300 rounded-md p-3 focus:outline-none focus:ring focus:ring-sky-300">Another</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const setup = ref("")
const delivery = ref("")
const joke = ref("")

const loadJoke = async () => {
  const request = await fetch('https://v2.jokeapi.dev/joke/christmas')
  const data = await request.json()

  setup.value = data.setup
  delivery.value = data.delivery
  joke.value = data.setup
}

const tellMe = () => {
  joke.value = delivery.value
}

loadJoke()
</script>
