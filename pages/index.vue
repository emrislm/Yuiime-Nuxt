<template>
  <div class="container">
    <input v-model="searchTerm" @keyup.enter="getAnimeBySearch" type="text" placeholder="Type here"
      class="input input-bordered w-full md:max-w-xs" />

    <div v-show="!isLoading" class="flex flex-row gap-4 overflow-x-scroll my-10">
      <div v-for="anime in animes" :key="anime.mal_id" class="card w-56 bg-base-100 shadow-xl image-full">
        <figure><img :src="anime.images.jpg.image_url" alt="" /></figure>
        <div class="card-body">
          <h2 class="card-title">{{ anime.title_english }}</h2>
          <div class="card-actions justify-end">
            <button class="btn btn-primary">Buy Now</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios';

const isLoading = ref(true);
const searchTerm = ref('');
const animes = ref([]);

const getAnimeBySearch = () => {
  const searchResult = axios.get(`https://api.jikan.moe/v4/anime?q=${searchTerm.value}`)
    .then((response) => { animes.value = response.data.data; })
    .catch((error) => { console.log(error); })
    .finally(() => { isLoading.value = false; });
};
</script>