<template>
    <div class="forecastSection">
        <h4>FORECAST</h4>
        <table v-if="weatherForecastData && weatherForecastData.forecast.forecastday">
            <thead>
                <tr>
                    <th></th>
                    <th>Date</th>
                    <th>Min-Temp</th>
                    <th>Max-Temp</th>
                    <th>Condition</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(day, index) in weatherForecastData.forecast.forecastday.slice(0, 7)" :key="index">
                    <td>
                        <img :src="day.day.condition.icon" alt="Weather Icon" class="weatherIcon" />
                    </td>
                    <td>{{ day.date }}</td>
                    <td>{{ displayMinTemp(day) }}</td>
                    <td>{{ displayMaxTemp(day) }}</td>
                    <td>{{ day.day.condition.text }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'ForecastSection',
    props: {
        weatherForecastData: {
            type: Object,
            required: true
        },
        units: {
            type: String,
            required: true
        }
    },
    data() {
        return {}
    },
    methods: {
        displayMinTemp(day) {
            return this.units === 'metric' ? `${day.day.mintemp_c}°C` : `${day.day.mintemp_f}°F`;
        },
        displayMaxTemp(day) {
            return this.units === 'metric' ? `${day.day.maxtemp_c}°C` : `${day.day.maxtemp_f}°F`;
        },
    }
};
</script>

<style scoped>
.forecastSection {
    background-color: rgba(0, 0, 0, 0.40);
    padding: 10px;
    width: 500px;
    box-sizing: border-box;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    box-shadow: 0 0px 6px rgba(0, 0, 0, 0.5);
    margin: 10px 0px 20px 0px;
    text-align: center;
}

.weatherImage {
    width: 70px;
    height: 70px;
    object-fit: cover;
    margin-bottom: 10px;
}

.weatherDate,
.minTemp,
.maxTemp {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

@media (max-width: 992px) {
    .weatherImage {
        width: 50%;
        /* Adjusted for responsiveness */
        height: auto;
    }

    .forecastSection {
        margin-right: 10px;
        width: 100%;
    }
}
</style>