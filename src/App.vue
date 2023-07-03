<script setup>
  import { nextTick, ref } from 'vue'
  import Weather from './components/Weather.vue'

  const city = ref('')
  const showWeather = ref()

  const searchWeather = async () => {
    showWeather.value = false
    await nextTick() // need to be explore
    showWeather.value = true
  }
</script>

<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">Weather Application</h1>

      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input v-model="city" type="text" class="input form-control" placeholder="Enter a City">
        </div>

        <button class="btn-search btn btn-primary" @click="searchWeather">Search <i class="fas fa-search"></i></button>
      </div>
    </div>
    <br>
    <Weather :city="city" v-if="showWeather"/>
  </div>
</template>

<style>
  body{
    background-color: #121212;
  }

  .header{
    background-color: #212730;
    border-radius: 20px;
    color: #fff;
    text-align: center;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    margin-top: 5rem;
  }

  .btn-search{
    background-image: linear-gradient(to right, cyan, magenta);
  }
</style>
