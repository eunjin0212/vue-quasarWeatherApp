<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input
        color="purple-12"
        v-model="search"
        placeholder="Search"
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon name="my_location" />
        </template>
        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>
    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">Manchester</div>
      <div class="text-h6 text-weight-light">Rain</div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>8</span>
        <span class="text-h4 relative-position degree">&deg;</span>
      </div>
      <div class="col text-center">
        <img src="https://www.fillmurray.com/100/100" alt="Bill" />
      </div>
      <div class="col skyline"></div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
    };
  },
});
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)
.degree
  top: -44px
.skyline
  background: url(../../town.png)
  background-size: contain
  background-position:  bottom
  height: 100px
</style>
/* flex-grow | flex-shrink | flex-basis */
