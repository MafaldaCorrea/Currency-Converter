<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>Currency Converter App</h1>
    <div class="container">
        <div class="container-source">
          <select name="source-currency" id="source-currency">
            <option value="CAD">CAD</option>
            <option value="DKK">DKK</option>
            <option value="GBD">GBD</option>
            <option value="EUR">EUR</option>
          </select>
          <input type="number" name="input-source" id="input-source">
        </div>
        <div class="container-info">
            <button>Switch</button>
            <div id="baseValue" class="h4">1 USD = 0.9194</div>
        </div>
        <div class="container-target">
          <select name="target-currency" id="target-currency">
            <option value="CAD">CAD</option>
            <option value="DKK">DKK</option>
            <option value="GBD">GBD</option>
            <option value="EUR">EUR</option>
          </select>
          <input type="number" name="target-source" id="target-source" placeholder="0" disabled>
        </div>
        <div class="container-update">
          <div class="last-updated h4">Last updated: 19th June 2021</div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  
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
    width: 50%;
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
