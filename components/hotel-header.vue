<template>
  <div class="header-container">
    <header class="hotel-header">
    <button v-if="this.isValidated" @click="this.logoutUser" class="logout-btn">Logout</button>
      <h1 class="overlook">Overlook Hotel</h1>
      <h2 class="overlook">Exclusive! Luxurious! Life-changing. . .</h2>
      <!-- <div class="logout"> -->
      <!-- </div> -->
    </header>
  </div>
</template>

<script>
export default {
  props: [ 'logoutUser', 'isValidated' ],
}
</script>

<style>
.hotel-header {
  background-color: #d7b56b;
  border: 2px solid #82724a;
  margin-bottom: 5px;
  height: 20vh;
}
.overlook {
  text-align: center;
}
.logout-btn {
  font-size: 20px;
  border-radius: 1rem;
  margin-top: 5px;
  margin-left: 143px;
}
</style>