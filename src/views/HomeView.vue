<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"></DataTitle>
    <DataBoxes :stats="stats"></DataBoxes>
    <CountrySelect @get-country="getCountryData" :countries="countries"></CountrySelect>
    <button 
      @click="clearCountryData()"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country
    </button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mt-10 mb-6">
      Fetching data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script lang="ts">

import axios from 'axios'
import { defineComponent } from 'vue';
import DataTitle from '@/components/DataTitle.vue';
import DataBoxes from '@/components/DataBoxes.vue';
import CountrySelect from '@/components/CountrySelect.vue';

export default defineComponent({
  name: 'HomeView',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data () {
    return {
      'loading': true,
      'title' : 'Global',
      'dataDate':'',
      'stats':{},
      'countries': [],
      'loadingImage': require('../assets/hour_glass.gif'),
      'covid_data': {},

    }
  },
  methods: {
    getCovidData(){
      axios.get('https://api.covid19api.com/summary')
            .then((response)=>{
              this.dataDate = response.data.Date;
              this.stats = response.data.Global;
              this.countries = response.data.Countries;
              this.loading = false;
            })
            .catch((e)=>{
              console.log(e);
              
            });
    },
    getCountryData(country: any){
      this.stats = country;
      this.title = country.Country;      
    },
    clearCountryData(){
      this.loading=true;
      this.getCovidData();
      this.title = 'Global';
    }
  },
  mounted () {
    this.getCovidData();
  }
});
</script>
