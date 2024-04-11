<template>
    <div class="weatherSection">
        <!-- <h4>WEATHER</h4> -->
        <div class="weatherContent" v-if="weatherData">
            <div class="weatherLeft">
                <img v-if="weatherData.current.condition.icon" :src="weatherData.current.condition.icon" class="weatherImage" alt="Weather Image" />
                <h1 class="temperatureDisplay">{{ temperatureDisplay }}</h1>
            </div>
            <div class="weatherRight">
                <h1 class="weatherName">{{ weatherData.location.name }}</h1>
                <p>{{ weatherData.location.region }}</p>
                <h3>{{ weatherData.location.country }}</h3>
            </div>
        </div>
        <div class="row">{{ weatherData.current.condition.text }}</div>
        <div class="row">Humidity: {{ weatherData.current.humidity }}%</div>
        <div class="row">Wind Speed: {{ weatherData.current.wind_mph }} mph</div>
    </div>
</template>

<script>
export default {
    name: 'WeatherSection',
    props: {
        weatherData: {
            type: Object,
            required: true
        },
        units: {
            type: String,
            required: true
        }
    },
    computed: {
        temperatureDisplay() {
            if (this.units === 'metric') {
                return `${this.weatherData.current.temp_c}°C`;
            } else {
                return `${this.weatherData.current.temp_f}°F`;
            }
        }
    },
    data() {
        return {}
    },
    methods: {

    }
};
</script>

<style scoped>
.weatherSection {
    background-color: rgba(0, 0, 0, 0.40);
    padding: 20px;
    width: 500px;
    box-sizing: border-box;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    box-shadow: 0 0px 6px rgba(0, 0, 0, 0.5);
    margin: 10px 10px 20px 0px;
    text-align: center;
    float: left;
}

.weatherImage {
    width: 170px;
    height: 170px;
    object-fit: cover;
    margin-bottom: 10px;
}

.weatherName {
            color: orangered;
            margin: 0;
            padding: 5px;
        }

.weatherContent {
    display: flex;
    width: 90%;
    justify-content: space-between;
    align-items: center;
}

.weatherLeft,
.weatherRight {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}

.row {
    padding: 10px;
    display: flex;
    justify-content: center;
}

.temperatureDisplay {
    margin: 0;
    padding: 0;
    margin-bottom: 40px;
    margin-left: 20px;
    margin-top: -10px;
}

@media (max-width: 992px) {
    .weatherSection {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        width: 100%;
    }
}
</style>