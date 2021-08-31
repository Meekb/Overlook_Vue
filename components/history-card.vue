<template>
  <div class="card-container">
    <h3>{{ date }}</h3>
    <p>{{ roomType }} #{{ roomNumber }}</p>
    <p>{{ bedSize }} x{{ numBeds }}</p>
    <p v-if="bidet">This room has a bidet!</p>
    <p>{{ total }}</p>
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
  width: 20%;
  margin: 5px 0 5px 0;
  border: 3px solid black;
}
</style>