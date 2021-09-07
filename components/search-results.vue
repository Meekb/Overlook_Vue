<template>
  <div class="results-container">
    <div 
      v-for="(item, i) in this.availability" 
      class="cards-container"
    >
      <result-card
        :key="i" 
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
  props: [ 'availability' ],
  data() {
    return {
      queryInfo: this.queryMatch,
    }
  },
  methods: {

  }
}
</script>

<style>
.results-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 25px;
  height: 27.5vh;
  overflow: scroll;
  background-color: black;
  padding: 25px;
  margin-right: 50px;
  width: 100%;
}
</style>