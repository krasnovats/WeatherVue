<template>
  <div class="weather" :class="weatherClass">
    <div class="container">

      <div class="card weather-form">
        <input type="text" name="" class="weather-form__input" v-model="searchQuery" @keyup.enter="weatherSearch" placeholder="Enter city">
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" v-show="!error && location && temperature !==0 && description">

      <div class="card" v-if="error">Error</div>

        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>

    </div>


    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="../src/assets/bg.jpg" alt="App Background">
        <img class="weather-bg__img light-rain" src="../src/assets/overcast.jpg" alt="App Background">
        <img class="weather-bg__img partly-cloudy" src="../src/assets/partly-cloudy.jpg" alt="Partly Cloudy">
        <img class="weather-bg__img sunny" src="../src/assets/sunny.jpg" alt="Sunny">
      </div>
    </div>

  </div>
</template>

<script>
 
export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },


  
  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny';
      } else if (this.description.includes('Light rain')) {
        return 'light-rain';
      } else if (this.description.includes('Partly cloudy')) {
        return 'partly-cloudy';
      } else {
        return '';
      }
    
    }
  },

  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`http://api.weatherapi.com/v1/current.json?key=8066e698f981433c9e8140804240112&q=${this.searchQuery}`)
      .then(response => response.json())
      .then(data => {
        this.loading = false;
        this.location = data.location.name;
        this.temperature = data.current.temp_c;
        this.description = data.current.condition.text;
        // this.resetSearchQuery();
      })
      .catch(error => {
        this.loading = false;
        this.error = true;
        console.error(error);
      })  
    }
  },
  resetSearchQuery() {
    this.searchQuery = '';
  }

}

</script>