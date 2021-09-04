<template>
   <div class="result">
      <h3>{{ this.type }} #{{ this.number }}</h3>
      <p v-if="this.numBeds < 2">{{ this.numBeds }} {{ this.bedSize }}</p>
      <p v-if="this.numBeds > 1">{{ this.numBeds }} {{ this.bedSize }}'s</p>
      <p v-if="bidet">This room has a bidet!</p>
      <p>${{ this.cost }}/night</p>
      <button class="book-btn">Book Room #{{ this.number }}</button>
    </div>
</template>

<script>
export default {
  props: [ 'bedSize', 'bidet', 'cost', 'number', 'numBeds', 'type' ],
  computed: {
    // bedFormat: function() {

    // } 
  },
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
  width: 75%;
  background-color: plum;
}
.book-btn {
  font-size: 18px;
  border-radius: 1rem;
}
</style>