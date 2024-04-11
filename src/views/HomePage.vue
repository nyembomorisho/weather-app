<template>
    <div class="logo">WEATHER FORECAST</div>
    <div class="container">
        <div>
            <search-section @update-weather="handleWeatherUpdate" @update-weather-forecast="handleWeatherForecastUpdate" @update-units="handleUnitUpdate" @favourites-updated="handleFavouritesUpdated" ></search-section>
        </div>
        <div>
            <div class="weather-forecast-container">
                <weather-section v-if="weatherData" :weatherData="weatherData" :units="selectedUnits"></weather-section>
                <forecast-section v-if="weatherForecastData" :weatherForecastData="weatherForecastData" :units="selectedUnits"></forecast-section>
            </div>
        </div>
        <div>
            <favourite-section :favourites="favourites" :units="selectedUnits"></favourite-section>
        </div>
    </div>
</template>

<script>
import SearchSection from '../components/SearchSection.vue'
import ForecastSection from '../components/ForecastSection.vue'
import WeatherSection from '../components/WeatherSection.vue'
import FavouriteSection from '../components/FavouriteSection.vue'

export default {
    components: {
        SearchSection,
        WeatherSection,
        ForecastSection,
        FavouriteSection
    },
    name: 'HomePage',
    props: {

    },
    data() {
        return {
            weatherData: null,
            weatherForecastData: null,
            favourites: [],
            selectedUnits: 'metric'
        };
    },
    methods: {
        handleWeatherUpdate(data) {
            this.weatherData = data;
        },
        handleUnitUpdate(unit) {
            this.selectedUnits = unit;
        },
        handleWeatherForecastUpdate(data) {
            this.weatherForecastData = data;
        },
        handleFavouritesUpdated(updatedFavourites) {
            this.favourites = updatedFavourites;
        }
    }
}
</script>

<style>
body {
    font-family: 'Figtree', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    min-height: 100vh;
    background-image: url('/src/assets/images/weather-background.jpg');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    background-repeat: no-repeat;
    margin: 0;
    overflow-x: hidden;
    position: relative;
}

.logo {
    position: absolute;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    color: white;
    font-size: 45px;
    font-family: "Bungee Spice", sans-serif;
    font-weight: 900;
    text-align: center;
    z-index: 1000;
}

.weather-forecast-container {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
}

@media (max-width: 992px) {
    .weather-forecast-container {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        width: 100%;
    }

    .logo {
        position: absolute;
        top: 20px;
        left: 50%;
        transform: translateX(-50%);
        color: white;
        font-size: 30px;
        font-family: "Bungee Spice", sans-serif;
        font-weight: 900;
        text-align: center;
        z-index: 1000;
    }
}
</style>
