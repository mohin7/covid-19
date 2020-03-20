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

      <div class="app-content">
        <div class="app-left-content">
          <div class="heading-top">
            <h1>Coronaviruses</h1>
            <p>Covid-19</p>
          </div>
          <!-- realtime report start  -->
          <div class="real-time-report">
            <h4>Realtime Report</h4>
            <div class="realtiem-report-wrapper">
              <div class="single-realtime-report">
                <h3>{{ worlData.cases || 'Lodaing' }}</h3>
                <p>Active</p>
              </div>
              <div class="single-realtime-report">
                <h3>{{ worlData.recovered || 'Lodaing' }}</h3>
                <p>Recovered</p>
              </div>
              <div class="single-realtime-report active">
                <h3>{{ worlData.deaths || 'Lodaing' }}</h3>
                <p>Death</p>
              </div>
            </div>
          </div>
          <!-- realtime report end -->

          <!-- video start  -->
          <div class="video-thumbnail">
            video
          </div>
          <!-- video end -->

          <div class="location-area">
            <select name="Select" @change="countryChange($event)">
              <option
                :value="country.country"
                v-for="country in countryAllData"
                :key="country.country"
                :selected="country.country == 'Bangladesh'"
                >{{ country.country || 'Lodaing' }}</option
              >
            </select>
            <div class="your-location">
              <div class="location">
                <h2>{{ countryData ? countryData.country : 'loading' }}</h2>
                <p>Description</p>
              </div>
              <button class="location-btn">
                <i class="fa fa-map-o" aria-hidden="true">fontawe</i>
              </button>
            </div>
            <div class="graphical-view">
              graph
            </div>
            
            <div class="location-effected">
              <ul>
                <li>
                  <strong>active</strong>
                  <span
                    >2542
                    <div class="status active"></div
                  ></span>
                </li>
                <li>
                  <strong>active</strong>
                  <span
                    >2542
                    <div class="status active"></div
                  ></span>
                </li>
                <li>
                  <strong>active</strong>
                  <span
                    >2542
                    <div class="status active"></div
                  ></span>
                </li>
              </ul>
            </div>
          </div>
        </div>
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
              <table>
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
                <tbody>
                  <tr
                    v-for="country in filteredCountrys"
                    :key="country.country"
                  >
                    <td>{{ country.country }}</td>
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
.header-area {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
}

.logo a {
  font-size: 30px;
  color: #333;
  font-weight: 700;
}

.menu-item ul li {
  display: inline-block;
}

.menu-item ul li a {
  color: #666;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: 500;
  display: block;
  padding: 10px 15px;
}

.header-right-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.menu-item ul {
  margin-bottom: 0;
}

.contact-button {
  background: #4228e6;
  color: #fff;
  border: none;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  padding: 10px 30px;
  border-radius: 50px;
  margin-left: 15px;
}

.heading-top h1 {
  font-size: 50px;
  font-weight: 400;
  line-height: 1;
}

.heading-top p {
  color: #666;
  font-size: 16px;
}

.real-time-report h4 {
  font-size: 18px;
}

.app-left-content {
  width: calc(100% - 40%);
}
.app-right-content{
  margin-left: 50px;
}
.app-content {
  display: flex;
  justify-content: space-between;
  margin-top: 70px;
}

.realtiem-report-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.single-realtime-report {
  background: #4228e6;
  margin-right: 8px;
  height: 180px;
  border-radius: 10px;
  padding: 15px;
  color: #fff;
  width: 100%;
}

.single-realtime-report:last-child {
  margin-right: 0;
}
.your-location {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>
