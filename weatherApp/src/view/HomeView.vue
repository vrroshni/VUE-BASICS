<template>
    <main class="container text-white">
        <div class="pt-4 mb-8 realtive">
            <input v-model="searchQuery" type="text" name="" placeholder="Search for a city or state" class="py-2 px-1 w-full bg-transparent border-b
                 focus:border-weather-secondary
                  focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]" @input="getSearchResults" id="">
            <ul v-if="mapboxSearchResults"
                class="absolute bg-weather-secondary text-white w-screen shadow-md py-2 px-1 top-[150]">
                <p class="py-2" v-if="searchError">
                    Sorry, something went wrong, please try again.
                </p>
                <p class="py-2" v-if="!searchError && mapboxSearchResults.length === 0">
                    No results match your query, try a different term.
                </p>
                <template v-else>
                    <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id" class="py-2 cursor pointer">
                        {{ searchResult.place_name }}
                    </li>
                </template>
            </ul>
        </div>

    </main>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';
const mapBoxAPIKey = 'pk.eyJ1Ijoicm9zaG5pdnIiLCJhIjoiY2xuNXNqZmx0MGQ3cDJxdDcydmE1dWlkeSJ9.OPuc0VpCiVl6rIRv7ABVLg'
const searchQuery = ref('')
const queryTimeout = ref(null)
const mapboxSearchResults = ref(null)
const searchError = ref(null)

const getSearchResults = () => {
    clearTimeout(queryTimeout.value)
    queryTimeout.value = setTimeout(async () => {
        if (searchQuery.value !== '') {
            try {
                const { data } = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapBoxAPIKey}&types=place`)
                mapboxSearchResults.value = data.features
                console.log(mapboxSearchResults.value)

            } catch (error) {
                searchError.value = true
            }
            return
        }

        mapboxSearchResults.value = null
    }, 300)

}
</script>

<style lang="scss" scoped></style>