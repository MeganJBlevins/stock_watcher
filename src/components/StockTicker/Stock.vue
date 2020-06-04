<template>
  <div v-if="this.stock" :class="{ stock__card: true, up: this.up, down: this.down }">
    <Slider />
    <div class="info">
      <div class="company__info">
        <h2 class="name">{{ this.stock.symbol }}</h2>
        <p class="symbol">{{ this.stock.symbol }}</p>
      </div>
      <div class="stock__info">
        <p class="close">{{ this.stock.close | formatNumber}}</p>
        <div class="difference">
          <DownArrow v-if="this.down" />
          <UpArrow v-if="this.up" />
          <p class="diff">{{ this.getDiff() | formatNumber}}</p>
          <p class="percentage">({{ this.getPercentChange() | formatNumber }}%)</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Numeral from 'numeral'
import DownArrow from '../Icons/DownArrow.vue'
import UpArrow from '../Icons/UpArrow.vue'
import Slider from './StockSlider.vue'

export default {
  name: 'Stock',
  props: {
    stock: {
      type: Object,
      default: null
    },
  },
  components: {
    DownArrow,
    UpArrow,
    Slider
  },
  data() {
    return {
      up: true,
      down: false,
      difference: 0
    }
  },
  filters: {
    formatNumber: function (number) {
    if (!number) return ''
    number = Numeral(number).format("0,0.00")
    return number
    }
  },
  mounted() {
    if(this.stock.open > this.stock.close) {
      this.up = false;
      this.down = true;
    }
  },
  methods: {
    getDiff() {
      return (this.stock.open - this.stock.close) * -1;
    },
    getPercentChange() {
      const changeValue = this.getDiff();
      return (changeValue / this.stock.open) * 100;
    }
  }
}
</script>