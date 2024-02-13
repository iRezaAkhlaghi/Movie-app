<template  >
    <div class="container h-[100vh] bg-gradient-to-r from-[#141E30] to-[#243B55] ">

  <div
            class="container min-h-screen flex flex-col justify-center items-center bg-gradient-to-b from-gray-800 to-gray-900 text-white p-8">

            <NuxtLink
                class="button text-lg font-semibold py-2 px-4 bg-blue-500 hover:bg-blue-600 rounded cursor-pointer mb-8"
                :to="{ name: 'index' }"> Back </NuxtLink>

            <div class="movie-info flex flex-col md:flex-row items-center md:items-start gap-8">
                <div class="movie-img">
                    <img :src="`https://image.tmdb.org/t/p/w500/${data.poster_path}`" alt=""
                        class="max-h-500 md:max-h-700 w-full md:w-auto" />
                </div>
                <div class="data-content text-left">
                    <h1 class="text-4xl md:text-5xl font-bold mb-4">{{ data.title }}</h1>
                    <p class="data-fact">
                        <span class="font-semibold">Released:</span>
                        {{ new Date(data.release_date).toLocaleString('en-us', {
                            month: 'long', day: 'numeric', year:
                                'numeric'
                        }) }}
                    </p>
                    <p class="data-fact">
                        <span class="font-semibold">Duration:</span> {{ data.runtime }} minutes
                    </p>
                    <p class="data-fact">
                        <span class="font-semibold">Original Language:</span>
                        {{ data.original_language }}
                    </p>
                    <p class="data-fact">
                        <span class="font-semibold">Overview:</span> {{ data.overview }}
                    </p>
                </div>
            </div>
        </div>









    </div>
</template>

<script setup>


import axios from 'axios';

let data = ref([])
let Movies = ref([])
let isLoading = ref(false)






const getSingle = async () => {
    try {
        isLoading.value = true; // Set loading to true before making the API request
        const response = await axios.get(
            `https://api.themoviedb.org/3/movie/${useRoute().params.id}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
        );



        if (response.data) {
            data.value = response.data;

            console.log(data.value);

        }







    } catch (error) {
        console.error('Error fetching movies:', error);
    } finally {
        isLoading.value = false; // Set loading to false after the API request is complete
    }
};


onMounted(getSingle);


</script>

<style lang="scss" scoped></style>