<template>
    <div class="searchSection">
        <h4>SEARCH</h4>
        <form @submit.prevent="getWeather">
            <input type="text" v-model="city" placeholder="Search for a place, city, or state" class="searchInput" />
            <div>
                <input type="radio" id="fahrenheit" value="imperial" v-model="units" class="tempRadio"
                    @change="this.getWeather()" />
                <label for="fahrenheit">Fahrenheit (F°)</label>

                <input type="radio" id="celsius" value="metric" v-model="units" class="tempRadio"
                    @change="this.getWeather()" />
                <label for="celsius">Celsius (C°)</label>
            </div>
            <div class="searchBtns">
                <button class="getWeatherBtn" @click.prevent="getWeather()">Get Weather</button>
                <button class="addToFavouriteBtn" @click.prevent="addToFavourites()">Add to Favourite</button>
                <button class="clearBtn" @click.prevent="clearSearch()">Clear Search</button>
            </div>
        </form>
        <p v-if="messageSuccess" style="color: green">{{ messageSuccess }}</p>
        <p v-if="messageError" style="color: red">{{ messageError }}</p>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'SearchSection',
    data() {
        return {
            WEATHER_API_KEY: "168f451897df4b4d81b175202241104",
            city: '',
            units: 'imperial', // Default to Fahrenheit
            messageSuccess: '',
            messageError: '',
            message: '',
            currentWeatherbaseURL: 'http://api.weatherapi.com/v1/current.json',
            forecastWeatherbaseURL: 'http://api.weatherapi.com/v1/forecast.json',
            weatherData: {}, // Will store the weather data fetched
            weatherForecastData: {},
            favourites: JSON.parse(localStorage.getItem('favourites')) || [],
        };
    },
    mounted() {
        this.getLocation(); // Call getLocation when the component is mounted
    },
    methods: {
        async getWeather() {
            this.clearMessages(); // Clear any previous messages
            if (!this.city.trim()) {
                this.messageError = 'Please enter a city name.';
                return;
            }

            try {
                const response = await axios.get(`${this.currentWeatherbaseURL}`, {
                    params: {
                        key: this.WEATHER_API_KEY,
                        q: this.city,
                        units: this.units
                    }
                });
                this.weatherData = response.data; // Save the fetched weather data
                this.$emit('update-weather', this.weatherData);
                this.$emit('update-units', this.units);
                this.setMessage('Weather data fetched successfully!', true);

                try {
                    const response = await axios.get(`${this.forecastWeatherbaseURL}`, {
                        params: {
                            key: this.WEATHER_API_KEY,
                            q: this.city,
                            days: 7, // get forecast for next 7 days
                            units: this.units
                        }
                    });
                    this.weatherForecastData = response.data; // Save the fetched weather data
                    this.$emit('update-weather-forecast', this.weatherForecastData);
                    this.$emit('update-units', this.units);
                    this.setMessage('Weather Forecast data fetched successfully!', true);
                } catch (error) {
                    console.error('Error fetching weather data:', error);
                    this.setMessage('Error fetching weather data: Please search for a VALID place, city, or state.', false);
                }
            } catch (error) {
                console.error('Error fetching weather data:', error);
                this.setMessage('Error fetching weather data: Please search for a VALID place, city, or state.', false);
            }
        },

        getWeatherForCity(city) {
            this.city = city;
            this.getWeather();
        },

        getCityNameFromCoordinates(latitude, longitude) {
            const apiKey = "AIzaSyD3-jzwfOd1RgbCIxOP2l8HXVTj9IbYwn4";
            const apiUrl = `https://maps.googleapis.com/maps/api/geocode/json?latlng=${latitude},${longitude}&key=${apiKey}`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    if (data.status === 'OK') {
                        const cityName = data.results[0].address_components.find(component => component.types.includes('locality')).long_name;
                        this.city = cityName; // Update Vue data property
                        this.getWeather();
                    } else {
                        console.error('Geocoding failed:', data.status);
                    }
                })
                .catch(error => console.error('Geocoding error:', error));
        },

        showError(error) {
            console.log("An error occurred with geolocation. Defaulting to New York.", error.message);
            this.getWeatherForCity('New York'); // Fetch weather for New York if there is an error
        },

        getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(position => {
                    this.getCityNameFromCoordinates(position.coords.latitude, position.coords.longitude);
                }, this.showError);
            } else {
                console.log("Geolocation is not supported by this browser. Defaulting to New York.");
                this.getWeatherForCity('New York');
            }
        },

        async addToFavourites() {
            if (!this.weatherData || this.weatherData.location.name.toLowerCase() !== this.city.toLowerCase()) {
                await this.getWeather();
            }

            const cityExists = this.favourites.some(fav => fav.name.toLowerCase() === this.city.toLowerCase());
            if (cityExists) {
                alert('This city has already been saved in your favourites.');
                return;
            }

            const favouriteData = {
                name: `${this.weatherData.location.name}`,
                temp: {
                    metric: `${this.weatherData.current.temp_c}°C`,
                    imperial: `${this.weatherData.current.temp_f}°F`
                },
                forecast: this.weatherData.current.condition.text,
                icon: this.weatherData.current.condition.icon
            };

            this.favourites.push(favouriteData);
            localStorage.setItem('favourites', JSON.stringify(this.favourites));
            alert('Success! Added to Favourites');

            // Emit an event to update the favourites in the parent component
            this.$emit('favourites-updated', this.favourites);
        },

        clearSearch() {
            this.city = '';
        },

        setMessage(message, isSuccess) {
            if (isSuccess) {
                this.messageSuccess = message;
            } else {
                this.messageError = message;
            }
            setTimeout(() => this.clearMessages(), 3000); // Clear the message after 3 seconds
        },

        clearMessages() {
            this.messageSuccess = '';
            this.messageError = '';
        }
    }
};
</script>

<style scoped>
.searchInput {
    background-color: rgba(255, 255, 255);
    color: black;
    font-size: 1.1em;
    border-radius: 15px;
    width: 80%;
    border: none;
    padding: 15px;
    margin-bottom: 15px;
}

.tempRadio+label {
    font-size: 1.1em;
}

.addToFavouriteBtn {
    background-color: orangered;
}

.addToFavouriteBtn:hover {
    background-color: #c13400;
}

.getWeatherBtn {
    background-color: black;
}

.getWeatherBtn:hover {
    background-color: #252525;
}

.searchBtns {
    margin: 15px;
    padding: 15px;
    display: inline;
    display: flex;
}

.clearFavouritesBtn {
    background-color: #878787;
}

.clearBtn {
    background-color: #878787;
}

.clearBtn:hover,
.clearFavouritesBtn:hover {
    background-color: green;
}

button {
    padding: 10px 20px;
    font-size: 1.1em;
    cursor: pointer;
    color: white;
    border: none;
    border-radius: 5px;
    margin-top: 5px;
    margin: 5px;
    outline: none;
}

.desktopLabel {
    display: inline;
}

.mobileLabel {
    display: none;
}

.weatherForm {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
}

.radioButtons,
.formButtons {
    display: flex;
    justify-content: center;
    margin-top: 10px;
}

.radioButtons input[type="radio"]+label {
    margin-right: 20px;
}

.searchSection {
    background-color: rgba(0, 0, 0, 0.40);
    padding: 20px;
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    box-shadow: 0 0px 6px rgba(0, 0, 0, 0.5);
    margin-top: 120px;
    text-align: center;
}

@media (max-width: 992px) {
    .searchSection {
        padding: 10px;
        margin-top: 150px;
        width: 500px;
    }

    .desktopLabel {
        display: none;
    }

    .mobileLabel {
        display: inline;
    }

    .getWeatherBtn,
    .addToFavouriteBtn,
    .clearBtn {
        font-size: 14px;
    }

    .searchSection,
    .weatherSection,
    .forecastSection,
    .favouriteSection {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }
}
</style>
