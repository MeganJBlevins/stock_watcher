<template>
  <div v-if="this.stock">
    <h2>{{ this.stock.symbol }}</h2>
  </div>
</template>
<script>
export default {
  name: 'Stock',
  props: {
    stock: {
      type: Object,
      default: null
    },
  }

}
</script>