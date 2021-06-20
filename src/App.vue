<template>
  <div id="app">
    <h1>Currency Converter App</h1>
    <div class="container">
        <div class="container-row container-source">
          <select name="source-currency" id="source-currency" v-model="source_currency" @change="calculateRate()">
            <option v-for="(currency, currencyCode) in currencies" :key="currency" :value="currencyCode">{{currencyCode}} ({{currency}})</option>
          </select>
          <input type="number" name="input-source" id="input-source" v-model="source_amount" @input="calculateRateFromSource()">
        </div>
        <div class="container-row container-info">
            <button class="button" @click="switchValues()">Switch</button>
            <div id="baseValue" class="h4">1 {{source_currency}} = {{rate}} {{target_currency}}</div>
        </div>
        <div class="container-row container-target">
          <select name="target-currency" id="target-currency" v-model="target_currency" @change="calculateRate()">
            <option v-for="(currency, currencyCode) in currencies" :key="currency" :value="currencyCode">{{currencyCode}} ({{currency}})</option>
          </select>
          <input type="number" name="target-source" id="target-source" placeholder="0" v-model="target_amount" @input="calculateRateFromTarget()">
        </div>
        <div class="container-update">
          <div class="container-row">
            <div class="last-updated h4">Last updated: {{data.date}}</div>
            <button class="button" @click="fetchData()">Update</button>
          </div>
        </div>
    </div>
    <p class="update-message" v-show="showUpdateMessage">{{updateMessage}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data:[],
      rates:[],
      currencies:[],
      rate: "",
      rawRate: "",
      source_currency: "EUR",
      target_currency: "USD",
      source_amount: 1,
      target_amount: 0,
      timestamp: "",
      apiKey : "9b6b363f0a5f9b1567602af4c61828dd",
      showUpdateMessage: false,
      updateMessage: "",
    }
  },

  methods: {
    fetchData(){
      fetch(`http://api.exchangeratesapi.io/v1/latest?access_key=${this.apiKey}`)
      .then(response => response.json())
      .then(data => {
        this.data = data;
        this.rates = data.rates;
        this.calculateRate();
        this.updateTimestamp();
      })
    },

    fetchCurrencies() {
      fetch(`http://api.exchangeratesapi.io/v1/symbols?access_key=${this.apiKey}`)
      .then(response => response.json())
      .then(data => {
        this.currencies = data.symbols;
      })
    },

    calculateRate() {
      this.rawRate = this.rates[this.target_currency] / this.rates[this.source_currency];
      this.rate = this.rawRate.toFixed(5);
      this.calculateRateFromSource();
    },
    
    calculateRateFromSource() {
      this.target_amount = (this.source_amount * this.rawRate).toFixed(2);
    },

    calculateRateFromTarget() {
      this.source_amount = (this.target_amount / this.rawRate).toFixed(2);
    },

    switchValues() {
      const sourceCurrency = this.source_currency;
      this.source_currency = this.target_currency;
      this.target_currency = sourceCurrency;
      this.calculateRate();
    },

    updateTimestamp() {
      if (this.timestamp) {
        this.showUpdateMessage = true;
        this.updateMessage = this.timestamp != this.data.timestamp ?
           "Currency rates have been updated" :
           "No changes to the currency rates"
        this.timestamp = this.data.timestamp;
        setTimeout(() => this.showUpdateMessage = false, 2000);
      } else {
        this.timestamp = this.data.timestamp;
      }
    }
  },

  mounted() {
    this.fetchData();
    this.fetchCurrencies();
  }
};
</script>

<style>
  html {
    background: #f8f8f8;
  }
  #app {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  }

  h1 {
    color: #35495e;
  }

  img {
    width: 150px;
  }

  .container {
    box-sizing: border-box;
    padding: 50px 20px;
    border-radius: 10px;
    background: #ebebeb;
    box-shadow: 0 5px 15px rgba(0,0,0,0.19), 0 6px 30px rgba(0,0,0,0.1);
    max-width: 450px;
  }

  .container-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .container-update {
    margin-top: 20px;
  }

  .button {
    padding: 5px;
    margin: 0 5px;
    font-size: 16px;
    background-color: #35495e;
    color: #ffffff;
    width: 30%;
    min-width: 80px;
    border: none;
    outline: none;
  }

  select {
    padding: 5px;
    margin: 10px 5px;
    border: 1px solid rgba(0,0,0,0.2);
    outline: none;
    height: 35px;
    box-sizing: border-box;
    width: 65%;
    font-size: 12px;
  }

  input {
    padding: 5px;
    margin: 10px 5px;
    border: 1px solid rgba(0,0,0,0.2);
    outline: none;
    font-size: 16px;
    height: 35px;
    box-sizing: border-box;
    width: 35%;
  }

  .h4 {
    font-weight: 500;
    font-size: 14px;
  }

  .update-message {
    background: #2a552091;
    font-size: 14px;
    padding: 10px;
    color: white;
    border: 1px dotted #2a5520e6;
    width: 300px;
    text-align: center;
    box-sizing: border-box;
  }

  @media (max-width: 450px) {
    .container {
      max-width: 350px;
    }

    .h4 {
      font-size: 12px;
    }

    h1 {
      font-size: 25px;
      text-align: center;
    }

    img {
      width: 100px;
    }
  }
</style>
