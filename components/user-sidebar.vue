<template>
  <section>
    <aside class="user-sidebar">
      <p class="your-history">
        Your History at Overlook
      </p>
      <h4 v-if="historyDisplayed">
        Total spent at Overlook: ${{ userTotal }}
      </h4>
      <button 
        v-if="!historyDisplayed" 
        @click.prevent="toggleUserHistory" 
        class="user-history-btn"
      >
        View History
      </button>
      <button 
        v-if="historyDisplayed" 
        @click.prevent="toggleUserHistory" 
        class="user-history-btn"
      >
        Hide History
      </button>
      <div v-if="historyDisplayed">
        <div 
          v-for="(item, index) in this.userHistory" 
          :key="index" 
          class="historyDisplay"
        >
          <history-card 
            :id="index" 
            :date="item.date" 
            :roomNumber="item.roomNumber" 
            :roomType="item.roomType" 
            :bedSize="item.bedSize" 
            :bidet="item.bidet" 
            :numBeds="item.numBeds" 
            :total="item.total" 
          />
        </div>
      </div>
      <div v-if="!historyDisplayed">
        <img class="sidebar-image" src="../static/overlook-skypool.png" width="275px"/>
      </div>
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
.sidebar-image {
  border-radius: 1.5rem;
  border: 5px solid #eaa654;
  margin-left: 0;
}
.your-history {
  font-size: 25px;
}
.user-history-btn {
  font-size: 18px;
  border-radius: 1rem;
  margin-top: 10px;
  margin-bottom: 25px;
}
</style>