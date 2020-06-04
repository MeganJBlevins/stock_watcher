<template>
  <div class="container">
   <h1>Stock Watcher</h1>
   <div class="stock__input">
     <form @submit.prevent="addStock">
       <input v-model="addStockSymbol" type="text" name="symbol"/>
       <button type="submit" >Add Stock</button>
     </form>
   </div>
   <div v-if="this.dataReady" class="stock__container">
     <div v-for="stock in stockData" :key="stock.symbol">
       <Stock 
        :stock="stock"
       />
     </div>
   </div>
  </div>
</template>

<script>
import axios from 'axios'
import Stock from './Stock.vue'

export default {
  name: 'StockContainer',
  components: {
    Stock
  },
  data() {
    return {
      dataReady: false,
      addStockSymbol: '',
      stocksVisible: ['IBM'],
      errors: [],
      apiEndpoint: 'https://www.alphavantage.co/',
      query: 'TIME_SERIES_MONTHLY_ADJUSTED',
      apiKey: 'HY0JP87WH3PG17X6',
      stockData: []
    }
  },
  async mounted() {
    i = 0;
    for (var i = 0; i < this.stocksVisible.length; i++) { 
      const symbol = this.stocksVisible[i];
      const endpointUrl = this.apiEndpoint + 'query?function=' + this.query + '&symbol=' + symbol + '&apikey=' + this.apiKey;
      // const endpointUrl = 'https://www.alphavantage.co/query?function=TIME_SERIES_WEEKLY_ADJUSTED&symbol=IBM&apikey=demo'
      console.log('getting stock info... ', symbol);
      try{
        let response = await axios.get(endpointUrl, { headers: {
          // remove headers
        }
        })
        const stock = {
          data: response.data["Monthly Adjusted Time Series"]['2020-06-04'],
        }
        const stockData = {
          symbol: symbol,
          open: stock['data']['1. open'],
          high: stock['data']['2. high'],
          low: stock['data']['3. low'],
          close: stock['data']['4. close']
        } 
        this.stockData.push(stockData)
        this.dataReady = true;
        console.log('stockData: ', this.stockData[symbol]);
      }catch(err){
        console.log('error: ', err)
      }
      console.log(this.stockData);
    }
  },
  methods: {
    addStock() {
      console.log('stock added', this.addStockSymbol)
    }
  }
}
</script>
