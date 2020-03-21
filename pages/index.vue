<template>
  <div class="container">
    <div class="app">
      <!-- header start  -->
      <div class="header-area">
        <div class="header-left-item">
          <div class="logo"><a href="#">DevsBD</a></div>
        </div>
        <div class="header-right-item">
          <div class="menu-item">
            <ul>
              <li><a href="#">About</a></li>
              <li><a href="#">Service</a></li>
            </ul>
          </div>

          <button class="contact-button">Contact</button>
        </div>
      </div>
      <!-- header end -->

      <div class="row mt-5">
        <div class="col-lg-7">
          <div class="app-left-content">
            <div class="heading-top">
              <h1>Coronaviruses <span>Last Update: </span></h1>
              <p>Covid-19</p>
            </div>
            <!-- realtime report start  -->
            <div class="real-time-report">
              <h4>Realtime Report (Global)</h4>
              <div class="realtiem-report-wrapper">
                <div class="single-realtime-report">
                  <h3>{{ worlData.cases || 'loading' }}</h3>
                  <p>Active</p>
                </div>
                <div class="single-realtime-report">
                  <h3>{{ worlData.recovered || 'loading' }}</h3>
                  <p>Recovered</p>
                </div>
                <div class="single-realtime-report active">
                  <h3>{{ worlData.deaths || 'loading' }}</h3>
                  <p>Death</p>
                </div>
              </div>
            </div>
            <!-- realtime report end -->

            <div class="location-area mt-4">
              <label for="#select">Selected  country: {{ countryData ? countryData.country : 'loading' }}</label>
              <select id="select" name="Select" @change="countryChange($event)">
                <option
                  :value="country.country"
                  v-for="country in countryAllData"
                  :key="country.country"
                  :selected="country.country == 'Bangladesh'"
                  >{{ country.country || 'Lodaing' }}</option
                >
              </select>

              <div class="location-effected">
                <ul>
                  <li>
                    <strong>Active</strong>
                    <span
                      >{{ countryData ? countryData.cases : 'loading' }}
                      <div class="status active"></div
                    ></span>
                  </li>
                  <li>
                    <strong>Today Effected</strong>
                    <span
                      >{{ countryData ? countryData.todayCases : 'loading' }}
                      <div class="status t-effected"></div
                    ></span>
                  </li>
                  <li>
                    <strong>Death</strong>
                    <span
                      >{{ countryData ? countryData.deaths : 'loading' }}
                      <div class="status death"></div
                    ></span>
                  </li>
                  <li>
                    <strong>Recovered</strong>
                    <span
                      >{{ countryData ? countryData.recovered : 'loading' }}
                      <div class="status recovered"></div
                    ></span>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
        <div class="col-lg-5">
          <div class="app-right-content">
            <div class="all-country">
              <div class="input-area">
                <input
                  type="text"
                  placeholder="Search here"
                  v-model="searchData"
                />
                <button class="search">
                  <i class="fa fa-search" aria-hidden="true"></i>
                </button>
              </div>
            
              <div class="country-list">
                <table class="table table-striped">
                  <thead>
                    <tr>
                      <td>Country</td>
                      <td>Active</td>
                      <td>Today Effect</td>
                      <td>Critical</td>
                      <td>Recover</td>
                      <td>Death</td>
                    </tr>
                  </thead>
                  <tbody class="custom-scroll">
                    <tr
                      v-for="country in filteredCountrys"
                      :key="country.country"
                    > <div v-if="filteredCountrys.length == 0">No data</div>
                      <td><strong>{{ country.country }}</strong></td>
                      <td>{{ country.cases }}</td>
                      <td>{{ country.todayCases }}</td>
                      <td>{{ country.critical }}</td>
                      <td>{{ country.recovered }}</td>
                      <td>{{ country.deaths }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      worlData: {},
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
@import "@/assets/css/style.css";
</style>
