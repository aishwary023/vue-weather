<template>
  <div id="app">
    <div
      id="bg"
      :class="
        typeof this.weather != 'undefined' && this.temp > 20
          ? 'container warm'
          : 'container'
      "
    >
      <main>
        <div class="search-box">
          <input
            type="text"
            id="tags"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keyup.enter="search"
          />
        </div>

        <div class="weather-wrap">
          <div class="location-box" v-if="city">
            <div class="location">{{ this.city }}, {{ this.country }}</div>
            <div class="date">
              <em>{{ this.dateBuilder() }}</em>
            </div>
          </div>

          <div class="weather-box" v-if="this.temp">
            <div class="temp">{{ this.temp }}°C</div>
            <div class="weather">{{ this.weather }}</div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      API_KEY: '05a4e04f9a1ca5c59fde495146b8f4cd',
      query: '',
      API_BASE_URL: 'https://api.openweathermap.org/data/2.5/weather?',
      city: '',
      country: '',
      date: '',
      temp: 0.0,
      weather: ''
    };
  },
  methods: {
    search() {
      axios
        .get(this.API_BASE_URL + 'q=' + this.query + '&appid=' + this.API_KEY)
        .then((response) => {
          console.log('X', response);
          this.city = response.data.name;
          this.country = response.data.sys.country;
          this.temp = (Number(response.data.main.temp) - 273.15).toFixed(0);
          this.weather = response.data.weather[0].main;
        });
    },
    dateBuilder() {
      var days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday'
      ];

      var months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December'
      ];
      var d = new Date();
      var day = days[d.getDay()];
      var date = d.getDate();
      var month = months[d.getMonth()];
      var year = d.getFullYear();
      var out = day + ' ' + date + ', ' + month + ' ' + year;
      return out;
    }
  }
};
</script>

<style>
body {
  background: rgb(230, 255, 255);
}

.container {
  width: 432px;
  height: 768px;
  margin-top: 4%;
  padding: 0;
}
@media only screen and (max-width: 600px) {
  .container {
    width: 100%;
    height: 100vh;
    margin-top: 0;
  }
}

.container {
  font-family: 'Montserrat', 'sans-serif';
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  transition: 0.5s;
  box-shadow: 10px 10px 12px 0px rgb(143, 143, 143);
}

main {
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.75),
    rgba(0, 0, 0, 0.25)
  );
  height: 100%;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  width: 100%;
  display: block;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);

  border-radius: 16px 0 16px 0;
  transition: 0.5s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 1);
  border-radius: 0px 16px 0px 16px;
}

main {
  text-align: center;
}
.location {
  font-size: 40px;
  color: #fff;

  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date {
  color: #fff;
  font-weight: 300;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;

  font-size: 102px;
  font-weight: 900;

  color: #fff;
  margin: 30px 0;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
}

#bg.warm {
  background-image: url('./assets/warm-bg.jpg');
}
</style>
