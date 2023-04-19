<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 289 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp-cel">{{ Math.round(weather.main.temp - 273.15) }}°c</div>
          <br>
          <div class="temp-far">{{ Math.round((weather.main.temp - 273.15) * 9/5 + 32) }}°F</div>

          <div class="weather">{{ weather.weather[0].main }}</div>
          <br>
          <div class="weather-description">{{ weather.weather[0].description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '6afc55c4c5670814274c409fb6d77755',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>

:root {
  --light: #e2eaf3;
  --dark: #2e3542;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Roboto Mono', monospace;
}

#app {
  /* background-image: url('./assets/cold-bg.jpg'); */
  background: var(--light);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background: #2e3542;
  /* background-image: url('./assets/warm-bg.jpg'); */
}
#app.warm .search-box .search-bar {
    
  color: var(--light);
  background: #2e3542;
box-shadow:  32px 32px 56px #252b35,
             -32px -32px 56px #373f4f;
}
#app.warm .search-box .search-bar:focus{
  background: #2e3542;
box-shadow: inset 32px 32px 56px #252b35,
            inset -32px -32px 56px #373f4f;
}
main {
  min-height: 100vh;
  padding: 25px;

  
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
  
}

.search-box .search-bar {
  font-family: 'Roboto Mono', monospace;
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: #e2eaf3;
box-shadow:  32px 32px 56px #b7bec5,
             -32px -32px 56px #ffffff;
  border-radius: 16px 16px 16px 16px;

}

.search-box .search-bar:focus {
  background: #e2eaf3;
box-shadow: inset 32px 32px 56px #b7bec5,
            inset -32px -32px 56px #ffffff;
  
}

.location-box .location {
  color: var(--dark);
  font-size: 32px;
  font-weight: 500;
  text-align: center;
 
}
#app.warm .location-box .location {
  color: var(--light);
}
.location-box .date {
  color: var(--dark);
  font-size: 20px;
  font-weight: 300;
  
  text-align: center;
}
#app.warm .location-box .date {
  color: var(--light);
}
.weather-box {
  text-align: center;
}

.weather-box .temp-cel, .weather-box .temp-far  {
  display: inline-block;
  padding: 10px 25px;
  color: var(--dark);
  font-size: 102px;
  font-weight: 900;

  background-color:rgba(255, 255, 255, 0.185);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
#app.warm .weather-box .temp-cel,
#app.warm .weather-box .temp-far {
  color: var(--light);
}
.weather-box .weather {
  color: var(--dark);
  font-size: 48px;
  font-weight: 700;
}
#app.warm .weather-box .weather{
  color: var(--light);
}
.weather-description{
  color: var(--dark);
  font-size: 24px;
  font-weight: 700;
}
#app.warm .weather-description{
  color: var(--light);
}
</style>