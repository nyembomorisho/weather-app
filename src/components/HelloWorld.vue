<template>
    <div class="logo">WEATHER FORECAST</div>
    <div class="container">
      <div class="searchSection">
        <search-section></search-section>
      </div>

      <div class="weather-forecast-container">
        <div class="weatherSection">
          <h4>WEATHER</h4>
          <div class="weatherContent">
            <div class="weatherLeft">
              <img id="weatherImage" src="" alt="Weather Image" />
              <h1 id="weatherDegree" class="weatherDegree"></h1>
            </div>
            <div class="weatherRight">
              <h1 id="weatherName" class="weatherName"></h1>
              <p id="weatherRegion" class="weatherRegion"></p>
              <h3 id="weatherCountry" class="weatherCountry"></h3>
            </div>
          </div>
          <div class="row" id="weatherDetail1"></div>
          <div class="row" id="weatherDetail2"></div>
          <div class="row" id="weatherDetail3"></div>
        </div>

        <div class="forecastSection">
          <h4>FORECAST</h4>
          <img id="weatherImage" src="" alt="Weather Image" />
          <p id="weatherDate" class="weatherDate"></p>
          <p id="minTemp" class="minTemp"></p>
          <p id="maxTemp" class="maxTemp"></p>
        </div>
      </div>

      <div class="favouriteSection">
        <h4>FAVOURITE</h4>
        <div id="favouritesList">
        </div>
        <div>
          <button id="clearFavouritesBtn" class="clearFavouritesBtn">Clear</button>
        </div>
      </div>
    </div>
</template>

<script>
import SearchSection from './SearchSection.vue'
export default {
  components: { SearchSection },
  name: 'HelloWorld'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
