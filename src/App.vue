<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>Currency Converter App</h1>
    <div class="container">
        <div class="container-source">
          <select name="source-currency" id="source-currency" v-model="source_currency" @change="calculateRate()">
            <option v-for="currency in currencies" :key="currency" :value="currency">{{currency}}</option>
          </select>
          <input type="number" name="input-source" id="input-source" v-model="source_amount" @input="calculateRate()">
        </div>
        <div class="container-info">
            <button @click="switchValues()">Switch</button>
            <div id="baseValue" class="h4">1 {{source_currency}} = {{rate}} {{target_currency}}</div>
        </div>
        <div class="container-target">
          <select name="target-currency" id="target-currency" v-model="target_currency" @change="calculateRate()">
            <option v-for="currency in currencies" :key="currency" :value="currency">{{currency}}</option>
          </select>
          <input type="number" name="target-source" id="target-source" placeholder="0" disabled v-model="target_amount">
        </div>
        <div class="container-update">
          <div class="last-updated h4">Last updated: {{data.date}}</div>
          <button>Update</button>
        </div>
    </div>
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
      source_currency: "CAD",
      target_currency: "USD",
      source_amount: 1,
      target_amount: 0,
      apiKey : "9b6b363f0a5f9b1567602af4c61828dd"
    }
  },

  methods: {
    fetchData(){
      fetch(`http://api.exchangeratesapi.io/v1/latest?access_key=${this.apiKey}`)
      .then(response => response.json())
      .then(data => {
        this.data = data;
        this.rates = data.rates;
        this.currencies = Object.keys(this.rates);
        this.calculateRate();
        console.log("here")
      })
    },

    calculateRate() {
        var rawRate = this.rates[this.target_currency] / this.rates[this.source_currency];
        this.rate = rawRate.toFixed(5);
        this.target_amount = (this.source_amount * rawRate).toFixed(2);
    },

    switchValues() {
      const sourceCurrency = this.source_currency;
      this.source_currency = this.target_currency;
      this.target_currency = sourceCurrency;
      this.calculateRate();
    }
  },

  mounted() {
    this.fetchData();
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
    align-content: center;
    width: 100%;
    height: 100%;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  }

  h1 {
    color: #35495e;
  }

  img {
    width: 150px;
  }

  .container {
    width: 50%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-content: center;
    align-items: center;
    text-align: center;
  }

  .container-info {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 60%;
  }

  .container-info button {
    padding: 5px;
    font-size: 16px;
    background-color: #35495e;
    color: #ffffff;
    width: 30%;
    min-width: 80px;
    height: 10%;
    border: none;
    outline: none;
  }

  select {
    padding: 5px;
    margin: 10px 5px;
    border: 1px solid rgba(0,0,0,0.5);
    outline: none;
    font-size: 14px;
    height: 35px;
    box-sizing: border-box;
  }

  input {
    padding: 5px;
    margin: 10px 5px;
    border: 1px solid rgba(0,0,0,0.5);
    outline: none;
    font-size: 16px;
    height: 35px;
    box-sizing: border-box;
  }

  .h4 {
    font-weight: 500;
  }

</style>
