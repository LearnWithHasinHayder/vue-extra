<script setup>
import { ref, reactive } from 'vue'
const joke = reactive({
  value: 'Chuck Norris can do a wheelie on a unicycle.'
})

function getANewJoke() {
  fetch('https://api.chucknorris.io/jokes/random')
    .then(response => response.json())
    .then(data => joke.value = data.value)
}

const countries = reactive([])

function getCountries() {
  fetch('https://restcountries.com/v3.1/all?fields=name,capital')
    .then(response => response.json())
    .then(data => {
      console.log(data)
      // for (const index in data) {
      //   countries.push({ name: data[index].name.common, capital: data[index].capital[0] })
      // }
      data.filter(country=>country.name.common.startsWith('B')).forEach(country => {
        countries.push({ name: country.name.common, capital: country.capital[0] })
      })
    }
    )
}
</script>

<template>
  <section class="flex flex-col items-center w-[500px]">
    <p>
      {{ joke.value }}
    </p>
    <button @click="getANewJoke()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-48 mt-5">
      Get a new joke
    </button>
    <div class="mt-5">
      <ul name="countries" id="countries">
        <li v-for="country in countries" :key="country.name" :value="country.name">
          {{ country.name }}  = {{ country.capital }}
        </li>
      </ul>
      <button @click="getCountries()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-48 mt-5">
        Get Countries
      </button>
    </div>
  </section>
</template>

<style scoped></style>
