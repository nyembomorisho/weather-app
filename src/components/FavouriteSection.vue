<template>
  <div class="favouriteSection">
    <h4>FAVOURITE</h4>
    <div v-if="favourites.length > 0">
      <table>
        <thead>
          <tr>
            <th>Weather</th>
            <th>City</th>
            <th>Temperature</th>
            <th>Condition</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(fav, index) in favourites" :key="index">
            <td><img :src="fav.icon" alt="Weather Icon" class="weatherIcon" /></td>
            <td>{{ fav.name }}</td>
            <td>{{ getTemperatureDisplay(fav.temp) }}</td>
            <td>{{ fav.forecast }}</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td style="color: red; font-size: 24px; cursor: pointer;" @click="deleteFavourite(index)">X</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-else>
      No Favourites Saved
    </div>
    <button v-if="favourites.length" @click="clearFavourites" class="clearFavouritesBtn">Clear</button>
  </div>
</template>

<script>
export default {
  name: 'FavouriteSection',
  props: {
    units: String,
  },
  data() {
    return {
      favourites: [],
    };
  },
  mounted() {
    this.loadFavourites();
    window.addEventListener('storage', this.handleStorageChange);
  },
  beforeUnmount() {
    window.removeEventListener('storage', this.handleStorageChange);
  },
  methods: {
    loadFavourites() {
      const favs = localStorage.getItem('favourites');
      if (favs) {
        this.favourites = JSON.parse(favs);
      }
    },
    handleStorageChange(event) {
      if (event.key === 'favourites') {
        this.loadFavourites();
      }
    },
    deleteFavourite(index) {
      this.favourites.splice(index, 1);
      localStorage.setItem('favourites', JSON.stringify(this.favourites));
    },
    getTemperatureDisplay(temp) {
      return this.units === 'metric' ? `${temp.metric}` : `${temp.imperial}`;
    },
    clearFavourites() {
      this.favourites = [];
      localStorage.setItem('favourites', JSON.stringify([]));
      this.loadFavourites();
    }
  }
};
</script>

<style scoped>
.clearFavouritesBtn {
  background-color: #878787;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  margin-top: 10px;
  cursor: pointer;
  border: none;
}

.clearFavouritesBtn:hover {
  background-color: green;
}

.favouriteSection {
  background-color: rgba(0, 0, 0, 0.40);
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: white;
  box-shadow: 0 0px 6px rgba(0, 0, 0, 0.5);
  margin: 10px 10px 20px 0px;
  text-align: center;
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
</style>
