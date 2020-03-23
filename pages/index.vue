<template>
  <div class="row mt-5">
    <div class="col-lg-6">
      <div class="app-left-content">
        <div class="heading-top">
          <h1>
            Coronaviruses
            <span
              >Last Updated:
              {{ worlData ? getData(worlData.updated) : 'loading' }}</span
            >
          </h1>
          <p>
            Coronaviruses (CoV) are a large family of viruses that cause illness
            ranging from the common cold to more severe diseases such as Middle
            East Respiratory Syndrome (MERS-CoV) and Severe Acute Respiratory
            Syndrome (SARS-CoV)
          </p>
        </div>
        <!-- realtime report start  -->
        <div class="real-time-report">
          <h4>Realtime Report (Global)</h4>
          <div class="realtiem-report-wrapper">
            <div class="single-realtime-report">
              <h3>{{ worlData.cases || 'loading' }}</h3>
              <h5 class="danger">+{{ todayAffected }} (Today)</h5>
              <p>Total Cases</p>
            </div>

            <div class="single-realtime-report active">
              <h3>{{ worlData.deaths || 'loading' }}</h3>
              <h5 class="danger">+{{ todayDeath }} (Today)</h5>
              <p>Deaths</p>
            </div>
            <div class="single-realtime-report">
              <h3>{{ worlData.recovered || 'loading' }}</h3>
              <p>Recovered</p>
            </div>
          </div>
        </div>
        <!-- realtime report end -->

        <div class="location-area mt-4">
          <label for="#select"
            >Selected country:
            {{ countryData ? countryData.country : 'loading' }}</label
          >
          <select id="select" name="Select" @change="countryChange($event)">
            <option
              :value="country.country"
              v-for="country in countryAllData"
              :key="country.country"
              :selected="country.country == 'Bangladesh'"
              >{{ country.country || 'Lodaing' }}</option
            >
          </select>
          <!-- <p>{{country.country.sort()}}</p> -->
          <div class="location-effected">
            <ul>
              <li>
                <strong> Country:</strong>
                <span
                  >{{ countryData ? countryData.country : 'loading' }}
                  <div class="status active"></div
                ></span>
              </li>
              <li>
                <strong>Total Cases:</strong>
                <span
                  >{{ countryData ? countryData.cases : 'loading' }}
                  <div class="status active"></div
                ></span>
              </li>
              <li :class="{ danger: countryData.todayCases > 0 }">
                <strong>Today Affected:</strong>
                <span
                  >{{ countryData.todayCases > 0 ? '+' : ''
                  }}{{ countryData ? countryData.todayCases : 'loading' }}
                  <div class="status t-effected"></div
                ></span>
              </li>
              <li :class="{ danger: countryData.todayDeaths > 0 }">
                <strong
                  >Today Death{{
                    countryData.todayDeaths > 1 ? 's' : ''
                  }}:</strong
                >
                <span
                  >{{ countryData.todayDeaths > 0 ? '+' : ''
                  }}{{ countryData ? countryData.todayDeaths : 'loading' }}
                  <div class="status t-effected"></div
                ></span>
              </li>
              <li>
                <strong>Deaths:</strong>
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
    <div class="col-lg-6">
      <div class="app-right-content">
        <div class="all-country">
          <div class="input-area">
            <input type="text" placeholder="Search here" v-model="searchData" />
            <button class="search">
              <i class="fa fa-search" aria-hidden="true"></i>
            </button>
          </div>

          <div class="country-list">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th class="fix-width">Country</th>
                  <th>Total Cases</th>
                  <th>Today Affected</th>
                  <th>Today Deaths</th>
                  <th>Critical</th>
                  <th>Recover</th>
                  <th>Deaths</th>
                </tr>
              </thead>
              <tbody class="custom-scroll">
                <tr v-for="country in filteredCountrys" :key="country.country">
                  <div v-if="filteredCountrys.length == 0">No data</div>
                  <td class="fix-width">
                    <strong>{{ country.country }}</strong>
                  </td>
                  <td>{{ country.cases }}</td>
                  <td :class="{ danger: country.todayCases > 0 }">
                    {{
                      country.todayCases == 0
                        ? country.todayCases
                        : '+' + country.todayCases
                    }}
                  </td>
                  <td :class="{ danger: country.todayDeaths > 0 }">
                    {{
                      country.todayDeaths == 0
                        ? country.todayDeaths
                        : '+' + country.todayDeaths
                    }}
                  </td>
                  <td>{{ country.critical }}</td>
                  <td>{{ country.recovered }}</td>
                  <td>{{ country.deaths }}</td>
                </tr>
              </tbody>
            </table>
            <h4 class="text-center">
              {{ filteredCountrys.length == 0 ? 'No data available' : '' }}
            </h4>
          </div>
        </div>
      </div>
      <div class="nb">
        <h4>NB: If you have any suggestion, feel free inform me.</h4>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import moment from 'moment'
export default {
  data() {
    return {
      worlData: {},
      countryAllData: [],
      searchData: '',
      countryData: ''
    }
  },
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
    },
    getData(data) {
      return moment(data).fromNow()
    }
  },
  computed: {
    filteredCountrys() {
      return this.countryAllData.filter(country => {
        return country.country
          .toLowerCase()
          .includes(this.searchData.toLowerCase())
      })
    },
    todayAffected() {
      let total = 0
      this.countryAllData.forEach(data => {
        total = total + parseInt(data.todayCases)
      })
      return total
    },
    todayDeath() {
      let total = 0
      this.countryAllData.forEach(data => {
        total = total + parseInt(data.todayDeaths)
      })
      return total
    }
  }
}
</script>
<style>
@import '@/assets/css/table.css';
@import '@/assets/css/style.css';
</style>
