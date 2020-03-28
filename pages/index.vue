<template>
  <div class="row mt-5">
    <div class="col-lg-6">
      <div class="app-left-content">
        <div class="heading-top">
          <h1>
            করোনা ভাইরাস
            <span
              >সর্বশেষ আপডেট:
              {{ worlData ? getData(worlData.updated) : 'loading' }}</span
            >
          </h1>
          <p>
            করোনাভাইরাস বলতে ভাইরাসের একটি শ্রেণীকে বোঝায় যেগুলি স্তন্যপায়ী
            প্রাণী এবং পাখিদেরকে আক্রান্ত করে। মানুষের মধ্যে করোনাভাইরাস
            শ্বাসনালীর সংক্রমণ ঘটায়। এই সংক্রমণের লক্ষণ মৃদু হতে পারে, অনেকসময়
            যা সাধারণ সর্দিকাশির ন্যায় মনে হয় (এছাড়া অন্য কিছুও হতে পারে,
            যেমন রাইনোভাইরাস), কিছু ক্ষেত্রে তা অন্যান্য মারাত্মক ভাইরাসের জন্য
            হয়ে থাকে, যেমন সার্স, মার্স এবং কোভিড-১৯। মানবদেহে সৃষ্ট
            করোনাভাইরাস সংক্রমণ এড়ানোর মত কোনো টিকা বা অ্যান্টিভাইরাল ওষুধ আজও
            আবিষ্কৃত হয়নি।
          </p>
        </div>
        <!-- realtime report start  -->
        <div class="real-time-report">
          <h4>বর্তমান বিশ্বে আক্রান্তের রিপোর্টঃ</h4>
          <div class="realtiem-report-wrapper">
            <div class="single-realtime-report">
              <h3>
                {{
                  worlData.cases
                    ? worlData.cases.toLocaleString('bn-BD')
                    : '' || 'loading'
                }}
              </h3>
              <h5 class="danger">
                +
                {{
                  todayAffected.toLocaleString
                    ? todayAffected.toLocaleString('bn-BD')
                    : '' || 'loading'
                }}
                (Today)
              </h5>
              <p>আক্রান্ত হয়েছে</p>
            </div>

            <div class="single-realtime-report active">
              <h3>
                {{
                  worlData.deaths
                    ? worlData.deaths.toLocaleString('bn-BD')
                    : '' || 'loading'
                }}
              </h3>
              <h5 class="danger">
                +
                {{ todayDeath ? todayDeath.toLocaleString('bn-BD') : '' }}
                (আজকে)
              </h5>
              <p>মারা গেছেন</p>
            </div>
            <div class="single-realtime-report">
              <h3>
                {{
                  worlData.recovered
                    ? worlData.recovered.toLocaleString('bn-BD')
                    : '' || 'loading'
                }}
              </h3>
              <p>সুস্থ হয়েছেন</p>
            </div>
          </div>
        </div>
        <!-- realtime report end -->

        <div class="location-area mt-4">
          <label for="#select"
            >আপনার সিলেক্টেট দেশঃ
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
                <strong>দেশঃ</strong>
                <span
                  >{{ countryData ? countryData.country : 'loading' }}
                  <div class="status active"></div
                ></span>
              </li>
              <li>
                <strong>মোট আক্রান্তঃ </strong>
                <span
                  >{{
                    countryData
                      ? countryData.cases.toLocaleString('bn-BD')
                      : 'loading'
                  }}
                  <div class="status active"></div
                ></span>
              </li>
              <li :class="{ danger: countryData.todayCases > 0 }">
                <strong>আজ আক্রান্ত হয়েছেনঃ</strong>
                <span
                  >{{ countryData.todayCases > 0 ? '+' : ''
                  }}{{
                    countryData
                      ? countryData.todayCases.toLocaleString('bn-BD') || 0
                      : 'loading'
                  }}
                  <div class="status t-effected"></div
                ></span>
              </li>
              <li :class="{ danger: countryData.todayDeaths > 0 }">
                <strong>আজ মারা গেছেনঃ</strong>
                <span
                  >{{ countryData.todayDeaths > 0 ? '+' : '' }}
                  {{
                    countryData
                      ? countryData.todayDeaths.toLocaleString('bn-BD') || 0
                      : 'loading'
                  }}
                  <div class="status t-effected"></div
                ></span>
              </li>
              <li>
                <strong>মারা গেছেনঃ</strong>
                <span
                  >{{
                    countryData
                      ? countryData.deaths.toLocaleString('bn-BD')
                      : 'loading'
                  }}
                  <div class="status death"></div
                ></span>
              </li>
              <li>
                <strong>সুস্থ হয়েছেনঃ</strong>
                <span
                  >{{
                    countryData
                      ? countryData.recovered.toLocaleString('bn-BD')
                      : 'loading'
                  }}
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
            <input
              type="text"
              placeholder="দেশের নাম দিয়ে খুজুন!"
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
                  <th class="fix-width">দেশ</th>
                  <th>আক্রান্ত হয়েছেন</th>
                  <th>আজ আক্রান্ত হয়েছেন</th>
                  <th>আজ মারা গেছেন</th>
                  <th>ক্রিটিকাল</th>
                  <th>সুস্থ হয়েছেন</th>
                  <th>মারা গেছেন</th>
                </tr>
              </thead>
              <tbody class="custom-scroll">
                <tr v-for="country in filteredCountrys" :key="country.country">
                  <div v-if="filteredCountrys.length == 0">
                    কোনো ডাটা পাওয়া যায় নাই
                  </div>
                  <td class="fix-width">
                    <strong>{{ country.country }}</strong>
                  </td>
                  <td>{{ country.cases.toLocaleString('bn-BD') }}</td>
                  <td :class="{ danger: country.todayCases > 0 }">
                    {{
                      country.todayCases == 0
                        ? country.todayCases
                        : '+' + country.todayCases.toLocaleString('bn-BD')
                    }}
                  </td>
                  <td :class="{ danger: country.todayDeaths > 0 }">
                    {{
                      country.todayDeaths == 0
                        ? country.todayDeaths
                        : '+' + country.todayDeaths.toLocaleString('bn-BD')
                    }}
                  </td>
                  <td>{{ country.critical.toLocaleString('bn-BD') }}</td>
                  <td>{{ country.recovered.toLocaleString('bn-BD') }}</td>
                  <td>{{ country.deaths.toLocaleString('bn-BD') }}</td>
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
        <h4>কোনো পরামর্শ থাকলে জানাতে ভুলবেন না।</h4>
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
    },
    formatNumber(num) {
      if (num) {
        return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
      }
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
        if (data.todayDeaths == null) {
          data.todayDeaths = 0
        }
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
@import url('https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@300;400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css?family=Baloo+Da+2:400,500,600,700,800&display=swap');
body {
  font-family: 'Baloo Da 2', cursive;
}
h1,
h2,
h3 {
  font-family: 'Hind Siliguri', sans-serif;
}
</style>
