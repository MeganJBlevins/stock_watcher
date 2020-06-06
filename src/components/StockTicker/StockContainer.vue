<template>
  <div class="container">
    <div class="toggle">
      <label for="dataInput">Data Type:</label>
      <select name="dataInput" v-model="dataInput" @change="updateInput">
        <option value="local">Local</option>
        <option value="api">API</option>
      </select>
    </div>
    <div v-if="this.loading">
      <h2>Loading...</h2>
    </div>
    <div v-else>
      <h1>Stock Watcher</h1>
      <div class="stock__input">
        <form @submit.prevent="addStock">
          <input v-model="addStockSymbol" type="text" required name="symbol" placeholder="Enter stock symbol"/>
          <button type="submit" >Add <span>Stock</span></button>
        </form>
        <div class="error" v-if="this.inputError">
         <p>Sorry, we couldn't find that Symbol, please try again.</p>
        </div>
      </div>
      <div class="stock__container">
        <transition v-for="stock in stockData" :key="stock.symbol">
          <Stock 
            :stock="stock"
          />
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Stock from './Stock.vue'
import moment from 'moment'
import StockSymbols from './../symbols.js'

export default {
  name: 'StockContainer',
  components: {
    Stock
  },
  data() {
    return {
      addStockSymbol: '',
      stocksVisible: ['IBM', 'BA', 'BAC'],
      errors: [],
      apiEndpoint: 'https://www.alphavantage.co/',
      query: 'TIME_SERIES_DAILY_ADJUSTED',
      apiKey: 'HY0JP87WH3PG17X6',
      stockData: null,
      apiStockData: null,
      dataInput: 'local',
      loading: false,
      today:'',
      inputError: false,
      localData: [
          {
            name: 'ALPHABET INC. CL C',
            symbol: 'GOOG',
            open: 691,
            high: 709.28,
            low: 689.47,
            close: 706.32
          },
          {
            name: 'YAHOO! INC',
            symbol: 'YHOO',
            open: 29.28,
            high: 29.66,
            low: 29.06,
            close: 29.27
          },
          {
            name: 'AMERICAN INTERN…',
            symbol: 'AIG',
            open: 52.06,
            high: 53.47,
            low: 52.28,
            close: 53.08
          },
          {
            name: 'VELOCITYSHARES 3…',
            symbol: 'UWTIF',
            open: 1.37,
            high: 1.74,
            low: 1.50,
            close: 1.61
          },
          {
            name: '3X INVERSE CRUDE',
            symbol: 'DWTIF',
            open: 1253.41,
            high: 297.50,
            low: 245.59,
            close: 253.41
          },
          {
            name: 'GROUPON INC',
            symbol: 'GRPN',
            open: 4.08,
            high: 4.13,
            low: 3.60,
            close: 3.74
          },
        ],
      StockSymbols
    }
  },
  async mounted() {
    this.today = moment().day(moment().day() >= 5 ? 5 :-2).format('YYYY-MM-DD');
    this.stockData = this.localData
  },
  methods: {
    getApiStockData() {
      this.loading = true;
      const initApiStocks = [];
      i = 0;
      for (var i = 0; i < this.stocksVisible.length; i++) { 
        const symbol = this.stocksVisible[i];
        console.log(symbol);
        this.getStockData(symbol)
      }
      this.loading = false;
       console.log('Init Api Stocks:74 = ', initApiStocks);
      return initApiStocks;

    },
    getStockData(symbol){
      let stockData = null
      const endpointUrl = this.apiEndpoint + 'query?function=' + this.query + '&symbol=' + symbol + '&apikey=' + this.apiKey;
      if(symbol in StockSymbols.symbols){
        try {
          axios.get(endpointUrl)
          .then(response => (
            this.stockData.push(this.updateApiData(response, symbol))
          ))
          .catch(error => {
            console.log(error)
            this.errored = true
          })
          .finally(() =>   console.log('stock updated'))
        } catch(e){
          console.log(e);
          this.inputError = true;
        }
        return stockData
      } else {
        this.inputError = true;
      }
      
    },
    updateApiData(response, symbol) {
       // console.log('response data: ', response)
      try {
        const stock = response.data['Time Series (Daily)'][this.today];
        const stockData = {
          name: StockSymbols.symbols[symbol.toString()],
          symbol: symbol,
          open: stock['1. open'],
          high: stock['2. high'],
          low: stock['3. low'],
          close: stock['4. close']
        }
        return stockData
      } catch(e) {
        console.log('unable to find in api', e);
        this.inputError = true
      }
    },
    addStock() {
      this.inputError = false;
      this.loading = true;
      if (this.dataInput == 'api'){
        this.getStockData(this.inputToUpper());
      } else {
        this.addDummyStock();
      }
      if(!this.inputError){
        this.stocksVisible += this.inputToUpper();
      }
      this.loading = false;
    },
    addDummyStock(){
      if(this.inputToUpper() in StockSymbols.symbols){
        this.stockData.push({
          name: StockSymbols.symbols[this.inputToUpper()],
          symbol: this.inputToUpper(),
          open: 29.28,
          high: 29.66,
          low: 29.06,
          close: 29.27
        })
      } else {
        this.inputError = true;
      }
    },
    updateInput(){
      if(this.dataInput == 'local') {
        this.stockData = this.localData
      } else {
        if (this.apiStockData == null){
          const initData = this.getApiStockData()
          this.apiStockData = initData
        } 
        this.stockData = this.apiStockData
      }
    },
    inputToUpper(){
      return this.addStockSymbol.toUpperCase();
    }
  }
}
</script>
