<template>
  <div class="w-full max-w-sm px-8 pt-5 pb-3 relative mx-auto my-auto rounded-xl shadow-lg bg-white">
    <div class="text-center p-1 pb-3 flex-auto justify-center leading-6">
      <h1 class="text-3xl font-bold pb-8">Currency converter</h1>
    </div>
    <div class="flex flex-col gap-y-6 mb-5">
      <div>
        <label for="amount" class="mr-3 block mb-1">Enter amount</label>
        <input type="number"
               id="amount"
               v-model="amount"
               class="relative outline-none py-2 px-2 bg-white text-sm text-gray-700 placeholder-gray-400 focus:outline-none focus:shadow-outline shadow appearance-none border rounded"/>
      </div>
      <div class="flex items-center justify-between -mx-2 relative">
        <div class="w-1/3 px-2">
          <label for="amount" class="mr-3">From:</label>
          <vueSelect
            @selectOption="setCurrency"
            @clearExchange="clearResults"
            :defaultOption="currency"
          />
        </div>
        <div class="w-1/3 px-2">
          <svg viewBox="0 0 95.547 95.547"
               style="enable-background:new 0 0 95.547 95.547;"
               xml:space="preserve"
               class="w-5 h-5 ml-9 mt-5">
            <path
              d="M6.982,34.295h58.922L55.037,45.164c-0.375,0.375-0.586,0.884-0.586,1.414c0,0.53,0.211,1.039,0.586,1.414l6.307,6.304    c0.391,0.391,0.902,0.586,1.414,0.586s1.022-0.195,1.414-0.586l25.451-25.451c0.78-0.781,0.78-2.047,0-2.828L64.17,0.562    c-0.75-0.75-2.078-0.75-2.828,0l-6.303,6.305c-0.781,0.781-0.781,2.047,0,2.828l10.864,10.867H6.982c-1.104,0-2,0.896-2,2v9.733    C4.982,33.4,5.877,34.295,6.982,34.295z"/>
            <path
              d="M88.564,61.229H29.643l10.865-10.867c0.781-0.78,0.781-2.047,0-2.828l-6.304-6.305c-0.375-0.375-0.884-0.586-1.414-0.586    c-0.53,0-1.039,0.211-1.414,0.586L5.924,66.683c-0.781,0.78-0.781,2.047,0,2.827l25.451,25.451    c0.391,0.391,0.902,0.586,1.414,0.586c0.512,0,1.023-0.195,1.414-0.586l6.306-6.304c0.375-0.375,0.586-0.884,0.586-1.414    s-0.211-1.038-0.586-1.413L29.643,74.963h58.922c1.104,0,2-0.896,2-2v-9.733C90.564,62.125,89.67,61.229,88.564,61.229z"/>
          </svg>
        </div>
        <div class="w-1/3 px-2">
          <label for="amount" class="mr-3">To:</label>
          <vueSelect
            @selectOption="setCurrencyToExchanged"
            @clearExchange="clearResults"
            :defaultOption="currencyToExchanged"
          />
        </div>
        <span v-show="showResults" class="text-xs absolute left-1/2 -bottom-4 translate-x-[-50%]">1 {{ currency }} - {{ exchange }} {{
            currencyToExchanged
          }}</span>
      </div>
    </div>
    <div v-show="showResults" class="text-center font-bold pt-4 pb-2">
      Exchanged : {{ amountExchanged.toFixed(2) }} {{ currencyToExchanged }}
    </div>
    <div class="flex justify-center">
      <button @click="fetch" class="bg-blue-500 rounded hover:bg-blue-800 focus:bg-blue-800 text-white py-3 px-6 font-bold">Get
        Exchange
      </button>
    </div>
    <div class="text-center pt-5 pb-1 text-sm">Last updated: {{ updated }}</div>
  </div>
</template>

<script>
import vueSelect from './partials/vue-select.vue'
import axios from 'axios'

export default {
  data () {
    return {
      APIkey: 'c9851f00-8e98-11ec-b9b5-81aeeb772ada',
      amount: 1,
      amountExchanged: 0,
      rate: 0,
      showResults: false,
      currency: "PLN",
      currencyToExchanged: 'USD',
      updated: '13-02-2022',
    }
  },
  setup () {
    return {}
  },
  components: {
    vueSelect
  },
  watch: {
    amount (parseValue) {
      if (!isNaN(parseValue)) {
        this.amount = parseFloat(parseValue.toFixed(2))
      }
    }
  },
  computed: {
    exchange () {
      return parseFloat(this.rate.toFixed(2))
    },
  },
  methods: {
    setCurrency (val) {
      this.currency = val
    },
    setCurrencyToExchanged (val) {
      this.currencyToExchanged = val
    },
    clearResults (val) {
      this.showResults = val
    },
    fetch () {
      axios
        .get(`https://freecurrencyapi.net/api/v2/latest?apikey=${this.APIkey}&base_currency=${this.currency}`)
        .then(response => {
          let date = new Date(response.data.query.timestamp * 1000)
          this.updated = date.getDate() + '/' + ('0' + (date.getMonth() + 1)).slice(-2) + '/' + date.getFullYear()
          this.rate = response.data.data[this.currencyToExchanged]
          this.amountExchanged = this.rate.toFixed(2) * this.amount
          this.showResults = true
        })
        .catch(error => {
          console.log("There was an error: " + error.response)
        })
    },
  }
}
</script>
