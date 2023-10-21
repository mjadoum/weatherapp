<template>
  <div class="weather-app">
    
    
    <div class="container">
  <div class="row">
    <div class="col-lg-3 text-center mt-2">
    
      <img src="https://static.vecteezy.com/system/resources/thumbnails/024/825/193/small/3d-weather-icon-day-with-rain-free-png.png" alt="" width="170" height="170" >
    </div>
    <div class="col-lg-9 text-start mt-2 intro">
      <h4>About:</h4>
      <p>Our Weather App, a standout project in our <b>Internet Technology</b>  class module (Coursework 2). Created by IT students, this app utilizes Vue.js, Bootstrap, JSON, and OpenWeatherMap to provide accurate weather data. Join us on a tech-savvy journey into meteorology. </p>
    </div>

  </div>
</div>




    <div class="form-inline">
    <form @submit.prevent="searchLocation" class="form-inline">
    <label for="weatherapp" class="form-label text-start">Enter Location :</label>
      <input
      @input="emptyInputError = ''"
        v-model="location"
        placeholder="For example - Glasgow"
        class="form-control mt-1"
        id="weatherapp"
      >
      <button type="submit" class="btn btn-primary ">
        Search
      </button>

    </form>
    </div>
     <!-- Error message element -->
    <div v-if="error && !emptyInputError" class="alert alert-danger mt-2">
      {{ error }}
    </div>

     <div v-if="emptyInputError" class="alert alert-danger mt-2">
      {{ emptyInputError }}
    </div>


      <div class="container-fluid">
      <div v-if="weatherData">
        <div class="row">
          <div class="col-12 col-md-4" v-for="(search, index) in searchHistory" :key="index">
            <div class="card mb-4">
              <ul class="list-group list-group-flush">
                <li class="list-group-item">
                  <h2>{{ search.name }} - <img v-if="search.countryFlag" :src="search.countryFlag" alt="Country Flag" style="width: 50px; height: 50px;"></h2>
                </li>
                <li class="list-group-item">
                  <p><span>Temperature</span>: {{ search.weatherData.main.temp }}°C</p>
                </li>
                <li class="list-group-item">
                  <p><span>Weather</span>: {{ search.weatherData.weather[0].description }}</p>
                </li>
                <li class="list-group-item">
                  <button @click="deleteSearch(index)" class="btn btn-danger">Delete</button>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      location: "",
      weatherData: null,
      countryFlag: null,
      emptyInputError: "",
      error: null,
      searchHistory: [] // Use an array to track unique searches
    };
  },
  name: "WeatherApp",
  methods: {
    async searchLocation() {
      if (this.location) {
        
        // Convert the location to lowercase for case-insensitive comparison
        const searchLocation = this.location.toLowerCase();

        // Check if the location is already in the search history
        const isDuplicate = this.searchHistory.some(
          (search) => search.name.toLowerCase() === searchLocation
        );

        if (!isDuplicate) {
          // Location is not a duplicate, proceed with fetching data
          const apiKey = "390257a6e477e86cf5a21e4386707e13";
          const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&units=metric&appid=${apiKey}`;
          try {
            const response = await fetch(url);
            if (response.status === 404) {
              this.error = "Location not found. Please check the spelling and try again.";
            } else {
              const data = await response.json();
              this.weatherData = data;
              this.error = null;

              // Fetch the country flag based on the sys.country code
              await this.fetchCountryFlag(data.sys.country);

              // Add the search location to the history
              this.searchHistory.push({ name: this.location, weatherData: data, countryFlag: this.countryFlag });
              this.location = ""; // Clear the input field
            }
          } catch (error) {
            console.error("Error fetching weather data:", error);
            this.error = "An error occurred while fetching weather data.";
          }
        } else {
          // Location is a duplicate
          this.error = "This location has already been searched.";
        }
      } else {
        this.emptyInputError = "Please enter a location.";
      }
    },
    deleteSearch(index) {
      // Remove the card from the searchHistory based on the index
      this.searchHistory.splice(index, 1);
    },
    async fetchCountryFlag(countryCode) {
      const flagUrl = `https://flagcdn.com/w320/${countryCode.toLowerCase()}.png`;
      try {
        const response = await fetch(flagUrl);
        if (response.ok) {
          this.countryFlag = flagUrl;
        } else {
          console.error('Country flag image not found.');
        }
      } catch (error) {
        console.error('Error fetching country flag:', error);
      }
    },
    clearErrors() {
      this.emptyInputError = "";
      this.error = "";
    }
  }
};
</script>



<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&family=Tilt+Neon&display=swap');

.form-control {
font-family: 'Tilt Neon', sans-serif;
  padding: 10px;
  margin:40px auto;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 40% !important; 
  border-radius:1px;
}
.intro{
font-family: 'Tilt Neon', sans-serif;
}
.intro p{
font-family: 'Quicksand', sans-serif;
}
label{
font-family: 'Quicksand', sans-serif;
font-size:23px;
font-weight:600;
text-align:left;
}

.alert-danger {
font-family: 'Quicksand', sans-serif;
margin:40px auto !important;
  background-color: #f8d7da;
  border: 1px solid #f5c6cb;
  color: #721c24;
  padding: 10px;
  border-radius: 5px;
  font-weight:600;
  font-size:18px;
  width:40%;
}
.card{
margin:25px auto;
font-family: 'Quicksand', sans-serif;
box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
border-radius:2px;
   
}
.card span,h2{
font-weight:600;
}
.card img{
border-radius:50px;
}

button{
font-family: 'Quicksand', sans-serif;
border-radius:2px;
}
 *,
html {
    margin: 0;
    padding: 0;
    box-sizing: border-box;

    }
    *,
*:before,
*:after {
	box-sizing: inherit;
}

</style>
