<template>
  <section class="sidebar-container">
    <aside class="user-sidebar">
      <h3>{{ name }}'s Overlook History</h3>
      <button @click.prevent="displayUserHistory" class="user-history-btn">View History</button>
      <section v-if="historyDisplayed" :v-for="userHistory in this.userHistory" class="historyDisplay">
        <h4>Total spent at Overlook: ${{ userTotal }}</h4>
        <history-card :date="userHistory.date" />
      </section>
    </aside>
  </section>
</template>

<script>
export default {
  props: [ 'name', 'userHistory', 'userTotal' ],
  data () {
    return {
      historyDisplayed: false,
    }
  },
  methods: {
    displayUserHistory (e) {
      e.preventDefault()
      this.historyDisplayed = true;
      console.log(this.userHistory)
    }
  },
}
</script>

<style>

</style>