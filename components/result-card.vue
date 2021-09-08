<template>
   <div class="result">
      <h3 class="history-card-title">{{ this.type }} #{{ this.number }}</h3>
      <p v-if="this.numBeds < 2">{{ this.numBeds }} {{ this.bedSize }} bed</p>
      <p v-if="this.numBeds > 1">{{ this.numBeds }} {{ this.bedSize }} beds</p>
      <p v-if="bidet">This room has a bidet!</p>
      <p>${{ this.cost }}</p>
      <button class="book-btn" @click="this.formatAndSubmitBooking">
        Book Room #{{ this.number }}
      </button>
    </div>
</template>

<script>
export default {
  props: [ 'bedSize', 'bidet', 'cost', 'date', 'number', 'numBeds', 'type' ],
  methods: {
    formatAndSubmitBooking() {
      const newBooking = {roomNumber: this.number}
      this.$emit('new-booking', {newBooking})
    },
  }
}
</script>

<style>
.result {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
  border: 2px solid black;
  width: 90%;
  background-color: #d7b56b;
  border-radius: 1rem;
}
.history-card-title {
  margin-bottom: 0;
}
.book-btn {
  font-size: 18px;
  border-radius: 1rem;
}
</style>