<template>
   <div class="result">
      <h3>{{ this.type }} #{{ this.number }}</h3>
      <p>More Room Info</p>
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
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 10px;
  border: 2px solid black;
  width: 75%;
}
</style>