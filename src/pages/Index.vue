<template>
  <q-page class="flex column" :class="bgClass">
    <div class="col q-pt-lg q-px-md">
      <q-input
        color="purple-12"
        v-model="search"
        placeholder="Search"
        @keyup.enter="getWeatherBySearch"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon @click="getLoction" name="my_location" />
        </template>
        <template v-slot:append>
          <q-btn round dense flat icon="search" @click="getWeatherBySearch" />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">{{ weatherData.name }}</div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img
          :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
        />
      </div>
    </template>
    <template v-else>
      <div class="col column text-white text-center">
        <div class="col text-h2 text-weight-thin">Quasar<br />Weather</div>
        <q-btn class="col" @click="getLoction" flat>
          <q-icon left size="3em" name="my_location" />
          <div v-text="msg"></div>
        </q-btn>
      </div>
    </template>
    <div class="col skyline"></div>
  </q-page>
</template>
<script>
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather?',
      apiKey: '4658b2072b0daa8e1e75d5bbd6358604',
      msg: 'Find my Location',
    };
  },
  methods: {
    getLoction() {
      this.$q.loading.show();
      if (this.$q.platform.is.electron) {
        axios.get('https://freegeoip.app/json/').then((resp) => {
          this.lat = resp.data.latitude;
          this.lon = resp.data.longitude;
          this.getWeatherByCoords();
        });
      } else {
        navigator.geolocation.getCurrentPosition((position) => {
          this.lat = position.coords.latitude;
          this.lon = position.coords.longitude;
          this.getWeatherByCoords();
        });
      }
    },
    getWeatherByCoords() {
      this.$q.loading.show();
      axios(
        `${this.apiUrl}lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then((res) => {
        this.weatherData = res.data;
        this.$q.loading.hide();
      });
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      axios(`${this.apiUrl}q=${this.search}&appid=${this.apiKey}`)
        .then((response) => {
          this.weatherData = response.data;
          this.$q.loading.hide();
        })
        .catch((error) => {
          if (error.response.status === 404) {
            this.$q.loading.hide();
            this.msg = 'Not Found City 😥';
          }
        });
    },
  },
  computed: {
    bgClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-night';
        }
        return 'bg-day';
      }
      return null;
    },
  },
});
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)
  &.bg-night
    background: linear-gradient(to bottom, #232526, #414345)
  &.bg-day
    background: linear-gradient(to bottom, #00b4db, #0083b0)
.degree
  top: -44px
.skyline
  background: url(../../town.png)
  background-size: contain
  background-position-y: 9px
  flex: 0 0 100px
</style>
/* flex-grow | flex-shrink | flex-basis */
