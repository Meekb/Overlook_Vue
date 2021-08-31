<template>
  <div class="card-container">
    <h3>{{ date }}</h3>
    <p>{{ roomType }} #{{ roomNumber }}</p>
    <p>{{ numBeds }} {{ bedSize }}'s</p>
    <p v-if="bidet">This room has a bidet!</p>
    <p>Room Total: ${{ total }}</p>
  </div>
</template>

<script>
export default {
  props: ['date', 'roomNumber', 'roomType', 'bedSize', 'bidet', 'numBeds', 'total'],
}
</script>


<style>
.card-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 80%;
  margin: 5px 0 5px 25px;
  border: 3px solid black;
}
</style>