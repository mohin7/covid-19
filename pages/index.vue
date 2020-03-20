<template>
  <div class="container">
    <h2>Covid-19</h2>
    <p>{{ worlData || 'Lodaing' }}</p>
    <!-- <p>Time {{ moment('20111031', 'YYYYMMDD').fromNow() }}</p> -->
    <input type="text" placeholder="Search!" v-model="searchData" />
    <select name="Select" @change="countryChange($event)">
      <option
        :value="country.country"
        v-for="country in countryAllData"
        :key="country.country"
        :selected="country.country == 'Bangladesh'"
        >{{ country.country || 'Lodaing' }}</option
      >
    </select>
    <h4>Country: {{ countryData ? countryData : 'loading' }}</h4>
    <ul>
      <li v-for="country in filteredCountrys" :key="country.country">
        {{ countryAllData.length > 0 ? country.country : 'loading' }} - ({{
          country.cases
        }})
      </li>
    </ul>
    <!-- <p>{{ countryData }}</p> -->
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      worlData: null,
      countryAllData: [],
      searchData: '',
      countryData: ''
    }
  },
  components: {},
  mounted() {
    this.fetchAllData()
    this.fetchCountryData()
    this.defaultCountry()
  },
  methods: {
    async fetchAllData() {
      let res = await axios.get('https://corona.lmao.ninja/all')
      this.worlData = res.data
    },
    async fetchCountryData() {
      let res = await axios.get('https://corona.lmao.ninja/countries/')
      this.countryAllData = res.data
    },
    async countryChange(e) {
      // get value
      const countryName = e.target.value
      let res = await axios.get(
        `https://corona.lmao.ninja/countries/${countryName}`
      )
      this.countryData = res.data
    },
    async defaultCountry() {
      let res = await axios.get(
        `https://corona.lmao.ninja/countries/Bangladesh`
      )
      this.countryData = res.data
    }
  },
  computed: {
    filteredCountrys() {
      return this.countryAllData.filter(country => {
        return country.country
          .toLowerCase()
          .includes(this.searchData.toLowerCase())
      })
    }
  }
}
</script>
<style>

</style>
