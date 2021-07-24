<template>
  <main v-if="!loading">
    <AppTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelect @get-country="getCountryData" :countries="countries"/>
    <button @click="clearCountryData" v-if="stats.Country" class="bg-red-900 text-white rounded p-3 mt-10 focus:outline-none hover:bg-red-500"> Clear Country </button>
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Loading data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script>
import AppTitle from '@/components/AppTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'

export default {
  name: 'Home',
  components: {
    AppTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/loading.gif'),

    }
  },
  methods: {
    async fetchCovidData(){
      const res = await fetch ('https://api.covid19api.com/summary')
      const data = await res.json()      
      // console.log(data)
      return data;
    },
    getCountryData(country){
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData(){
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'GLobal'
      this.stats = data.Global
      this.loading = false
    }
  },
  async created(){
    const data = await this.fetchCovidData()
    
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
