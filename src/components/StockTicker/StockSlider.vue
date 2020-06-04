<template>
  <div class="slider__container">
    <div class="slider">
      <div 
        class="slider__pointer"
        :style="{ top: this.difference + '%' }">
      </div>
      <div class="slider__background"></div>
    </div>
    <div class="labels">
      <div class="high">
          <p>{{ this.max }}</p>
      </div>
      <div class="low">
          <p>{{ this.min }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "StockSlider",
  props: {
    min: {
      type: String,
      default: '0'
    },  
    max: {
      type: String,
      default: '100'
    },
    current: {
      type: String, 
      default: '50'
    },
    difference: {
      type: String,
      default: '0'
    }
  }
}
</script>