<template>
   <div class="result">
      <h3>{{ this.type }} #{{ this.number }}</h3>
      <p>Room Number</p>
      <p>ETC...</p>
      <button @click="consoleTheThing">ConsoleLog</button>
    </div>
</template>

<script>
export default {
  props: [ 'type', 'number' ],
  methods: {
    consoleTheThing() {
      console.log(this.type, this.number)
    }
  }
}
</script>

<style>
.result {
  border: 2px soild black;
}
</style>