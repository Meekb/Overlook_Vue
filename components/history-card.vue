<template>
  <div :id="id" class="card-container">
    <h3>{{ date }}</h3>
    <p>{{ roomType }} #{{ roomNumber }}</p>
    <p>{{ this.displayNumBeds() }}</p>
    <p v-if="bidet">This room had a bidet!</p>
    <p>Room Total: ${{ total }}</p>
  </div>
</template>

<script>
export default {
  props: ['date', 'id', 'roomNumber', 'roomType', 'bedSize', 'bidet', 'numBeds', 'total'],
  methods: {
    displayNumBeds () {
      if (this.numBeds > 1) {
        return `${this.numBeds} ${this.bedSize}'s`
      }
      return `${this.numBeds} ${this.bedSize}` 
    }
  }
}
</script>


<style>
.card-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 275px;
  margin: 5px 0px 5px 20px;
  border: 3px solid black;
  background-color: #d7b56b;
  border-radius: 1rem;
  font-weight: bold;
}
</style>