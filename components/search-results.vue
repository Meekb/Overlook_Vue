<template>
  <div :v-for="queryMatch in this.queryMatch" class="results-container">
    <div class="result">
      <h3>{{ queryMatch.number }} #{{ queryMatch.roomType }}</h3>
      <p>Room Number</p>
      <p>ETC...</p>
    </div>
  </div>
</template>

<script>
export default {
  props: [ 'queryMatch' ],
}
</script>

<style>
.results-container {
  display: grid;
  grid-template-columns: auto;
  /* grid-gap: 25px; */
}
</style>