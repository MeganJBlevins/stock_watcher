<template>
  <div id="app">
    <StockContainer/>
  </div>
</template>

<script>
import StockContainer from './components/StockTicker/StockContainer.vue'
import './assets//scss/main.scss'


export default {
  name: 'App',
  components: {
    StockContainer
  }
}
</script>

