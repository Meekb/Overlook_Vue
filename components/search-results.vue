<template>
  <div class="results-container">
    <div 
      v-for="item in this.queryInfo" 
      class="cards-container"
    >
      <result-card 
        :type="item.roomType" 
        :number="item.number" 
        :bidet="item.bidet" 
        :cost="item.costPerNight" 
        :numBeds="item.numBeds" 
        :bedSize="item.bedSize" 
      />
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
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 25px;
  height: 28vh;
  overflow: scroll;
  background-color: black;
  padding: 25px;
}
</style>