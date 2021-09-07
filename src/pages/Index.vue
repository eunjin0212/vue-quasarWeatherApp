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
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
    };
  },
});
</script>
<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)
</style>
