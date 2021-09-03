<template>
  <div class="results-container">
    <div>
      <h3>Here are our available rooms. Select one to continue:</h3>
    </div>
    <div v-for="item in this.queryInfo">
      <result-card :type="item.roomType" :number="item.number" />
    </div>
  </div>
</template>

<script>
import ResultCard from './result-card.vue'
export default {
  components: { ResultCard },
  props: [ 'queryMatch' ],
  data() {
    return {
      queryInfo: this.queryMatch,
    }
  },
}
</script>

<style>
.results-container {
  display: grid;
  grid-template-columns: auto;
  /* grid-gap: 25px; */
}
</style>