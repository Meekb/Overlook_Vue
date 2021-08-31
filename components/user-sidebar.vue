<template>
  <section class="sidebar-container">
    <aside class="user-sidebar">
      <h3>Your History at Overlook</h3>
      <h4 v-if="historyDisplayed">Total spent at Overlook: ${{ userTotal }}</h4>
      <button v-if="!historyDisplayed" @click.prevent="toggleUserHistory" class="user-history-btn">View History</button>
      <button v-if="historyDisplayed" @click.prevent="toggleUserHistory" class="user-history-btn">Hide History</button>
      <section v-if="historyDisplayed" v-for="(item, index) in this.userHistory" class="historyDisplay">
        <history-card :key="index" :id="index" :date="item.date" :roomNumber="item.roomNumber" :roomType="item.roomType" :bedSize="item.bedSize" :bidet="item.bidet" :numBeds="item.numBeds" :total="item.total" />
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
    toggleUserHistory (e) {
      e.preventDefault()
      this.historyDisplayed = !this.historyDisplayed;
    }
  },
}
</script>

<style>
.sidebar-container {
  width: 25%;
  /* vh here for desktop skeleton */
  height: 81vh;
  overflow: scroll;
  border: 2px solid black;
  text-align: center;
}
.user-history-btn {
  font-size: 18px;
  border-radius: 1rem;
}
</style>