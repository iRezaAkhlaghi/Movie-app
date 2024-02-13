<template>
    <div>
        <Head>
      <Title>Movie App - Latest Streaming Movie Info</Title>
      <Meta name="description" content="Get all the latest streaming movies in theaters & online"/>
      <Meta name="keywords" content="movies, stream, stremaing"/>
    </Head>




        <Hero />

        <!-- Movies -->
        <div class="container">

            <h2 class="font-bold text-yellow-500 text-4xl text-center mt-10">NEW MOVIES</h2>

            <div class="p-4">
                <input v-model="searchInput" @input="searchMovies" placeholder="Search"
                    class="border rounded p-2 w-full focus:outline-none focus:ring focus:border-blue-300" />

                <div v-if="searchInput.length" class="mt-4">
                    <h2 class="text-xl font-bold mb-2">Search Results:</h2>

                 
                </div>


                <div v-if="searchedMovies.length === 0" class="mt-4">
                    <p class="text-gray-500">Please enter a movie name to search.</p>
                </div>






            </div>


        </div>


        <div>

            <div class="grid grid-cols-4 gap-4 py-10">




                <div v-if="searchInput.length" v-for="(movie, index) in searchedMovies" :key="index"
                    class="relative overflow-hidden transition-transform transform hover:scale-[1.03]">

                    <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="Movie Poster"
                        class="w-full  object-cover">

                    <div
                        class="absolute inset-0 flex flex-col justify-end p-4 bg-black bg-opacity-75 opacity-0 hover:opacity-100 transition-opacity">
                        <div class="flex items-start">
                            <div class="bg-yellow-500 text-white font-bold px-2 py-1 rounded-md mb-2">{{  movie.vote_average.toFixed(2).slice(0, 3)  }}</div>
                        </div>
                        <p class="text-2xl font-semibold text-white mb-2">{{ movie.title.slice(0, 20) }}<span
                                v-if="movie.title.length > 20" class="text-gray-500">...</span></p>
                        <p class="text-gray-300 text-sm">{{ movie.overview }}</p>
                        <p class="text-gray-400 text-sm mt-2">
                            Released:
                            {{
                                new Date(movie.release_date).toLocaleString('en-us', {
                                    month: 'long',
                                    day: 'numeric',
                                    year: 'numeric',
                                })
                            }}
                        </p>
                        <NuxtLink :to="`/movies/${movie.id}`" class="text-blue-400 hover:text-blue-600 mt-2">
                            Get More Info
                        </NuxtLink>
                    </div>

                </div>

              



                <div v-else v-for="(movie, index) in Movies"
                    class="relative overflow-hidden transition-transform transform hover:scale-[1.03]">

                    <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="Movie Poster"
                        class="w-full  object-cover">

                    <div
                        class="absolute inset-0 flex flex-col justify-end p-4 bg-black bg-opacity-75 opacity-0 hover:opacity-100 transition-opacity">
                        <div class="flex items-start">
                            <div class="bg-yellow-500 text-white font-bold px-2 py-1 rounded-md mb-2">{{  movie.vote_average.toFixed(2).slice(0, 3)  }}</div>
                        </div>
                        <p class="text-2xl font-semibold text-white mb-2">{{ movie.title.slice(0, 20) }}<span
                                v-if="movie.title.length > 20" class="text-gray-500">...</span></p>
                        <p class="text-gray-300 text-sm">{{ movie.overview }}</p>
                        <p class="text-gray-400 text-sm mt-2">
                            Released:
                            {{
                                new Date(movie.release_date).toLocaleString('en-us', {
                                    month: 'long',
                                    day: 'numeric',
                                    year: 'numeric',
                                })
                            }}
                        </p>
                        <NuxtLink :to="`/movies/${movie.id}`" class="text-blue-400 hover:text-blue-600 mt-2">
                            Get More Info
                        </NuxtLink>
                    </div>

                </div>

            </div>


            <Loading v-if="isLoading" class="flex justify-center" />

        </div>





    </div>
</template>

<script setup>
import axios from 'axios';

let data = ref([])
let data2 = ref([])
let isGet = ref(false)
let Search = ref([])
let Movies = ref([])
let isLoading = ref(false)





const fetchData = async () => {
    try {
        isLoading.value = true;
        const response = await axios.get(
            `https://api.themoviedb.org/3/movie/now_playing?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1`
        );

        if (response.data) {
            data.value = response.data;
            Movies.value = data.value.results;
        }
    } catch (error) {
        console.error('Error fetching movies:', error);
    } finally {
        isLoading.value = false;
    }
};

const getMovies = async () => {
    await fetchData();
};

onMounted(async () => {
    await getMovies();
});




const searchedMovies = ref([]);
let searchInput = ref('')
let show = searchInput

const fetchSearchResults = async () => {
    try {
        isLoading.value = true;
        const response = await axios.get(
            `https://api.themoviedb.org/3/search/movie?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1&query=${searchInput.value}`
        );

        if (response.data) {
            searchedMovies.value = response.data.results;
        }

        console.log('Response:', response.data.results[0]);
    } catch (error) {
        console.error('Error fetching search results:', error);
    } finally {
        isLoading.value = false;
    }
};

const searchMovies = async () => {
    await fetchSearchResults();
};



watchEffect(async () => {
    if (searchInput.value === '') {
        await getMovies();
    } else {
        await searchMovies();
    }
});








</script>

<style lang="scss" scoped></style>