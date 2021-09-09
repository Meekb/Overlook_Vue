<template>
  <div class="results-container">
    <div v-if="!search" class="img-container">
      <img class="room-pic" src="../static/OverlookQueen.png" width="300px" />
      <img class="room-pic" src="../static/overlook-suite.png" width="300px" />
    </div>
    <div 
      v-for="(item, i) in this.availability" 
      class="cards-container" :key="i" 
    >
      <result-card
        @new-booking="sendReservation"
        :type="item.roomType" 
        :number="item.number" 
        :bidet="item.bidet" 
        :cost="item.costPerNight" 
        :numBeds="item.numBeds" 
        :bedSize="item.bedSize"
        :date="item.checkinDate" 
      />
    </div>
      <p v-if="search && !availability.length" class="no-availability">
        Ah, shit. We don't have any rooms of that type available.
        Please select another room type, or adjust your search date.
      </p>
  </div>
</template>

<script>
import ResultCard from './result-card.vue'
export default {
  components: { ResultCard },
  props: [ 'availability', 'search' ],
  data() {
    return {
      queryInfo: this.queryMatch,
      newReservation: undefined,
    }
  },
  methods: {
    sendReservation(payload) {
      this.newReservation = payload.newBooking
      const newReservation = this.newReservation
      this.$emit('new-reservation', {newReservation})
    }, 
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
  background-color: #82724a;
  padding: 25px;
  width: 920px;
  border-radius: 1.5rem;
  border: 5px solid #eaa654;
}
.room-pic {
  border: 5px solid #eaa654;
}
.img-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.no-availability {
  width: 300%;
  font-size: 25px;
  padding: 25px;

}
</style>