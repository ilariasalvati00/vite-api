<script>
import axios from 'axios'
import FilterAPI from './components/FilterAPI.vue'
import Brewery from "./components/Brewery.vue"
const api_path = "https://api.openbrewerydb.org/v1/breweries?by_country=poland&per_page=10"

export default {
  components: {
    Brewery,
    FilterAPI
  },
  data() {
    return {
      breweries_list: [],
      done: false,
    };
  },
  methods: {
    cambia_visualizzazione: function () {
      this.done = false;
    }
  },
  mounted: function () {
    axios.get(api_path).then((risposta) => {
      this.breweries_list = risposta.data;
      this.done = true;
    })
  }
}
</script>

<template>
  <div>
    <FilterAPI @update:search_loaded="cambia_visualizzazione()"></FilterAPI>
  </div>
  <div v-if="done == true">
    <Brewery v-for="(birreria) in breweries_list" :nome="birreria.name" :citta="birreria.city"
      :provincia="birreria.state_province" :indirizzo="birreria.street" :stato="birreria.country"
      :tipo="birreria.brewery_type"></Brewery>
  </div>
</template>

<style scoped></style>
