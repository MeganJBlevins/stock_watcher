<template>
  <div v-if="this.stock" :class="{ stock__card: true, up: this.up, down: this.down }">
    <Slider
      :min="this.stock.low | formatNumber"
      :max="this.stock.high | formatNumber"
      :current="this.stock.close | formatNumber"
      :difference="this.getDailyDifference()"
    />
    <div class="info">
      <div class="company">
        <h2 class="name">{{ this.getName() }}</h2>
        <p class="symbol">{{ this.stock.symbol }}</p>
      </div>
      <div class="totals">
        <p class="close">{{ this.stock.close | formatNumber}}</p>
        <div class="difference">
          <DownArrow v-if="this.down" />
          <UpArrow v-if="this.up" />
          <p class="diff">{{ this.getDiff() | formatNumber}}</p>
          <p class="percentage">({{ this.getPercentChange() | formatNumber }}%)</p>
        </div>
      </div>
      <div class="daily">
        <p class="open">Open <span>{{ this.stock.open | formatNumber }}</span></p>
        <p class="high">High <span>{{ this.stock.high | formatNumber }}</span></p>
        <p class="low">Low <span>{{ this.stock.low | formatNumber }}</span></p>
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
    getName() {
      let name = this.stock.name;
      if (name == undefined){
        name = this.stock.symbol;
      }
      return name
    },
    getDiff() {
      return (this.stock.open - this.stock.close) * -1;
    },
    getPercentChange() {
      const changeValue = this.getDiff();
      return (changeValue / this.stock.open) * 100;
    },
    getDailyDifference() {
      const dailyChangeRange = this.stock.high - this.stock.low;
      const diff = (dailyChangeRange / this.stock.low) * 100;
      return diff.toString();
    }
  }
}
</script>