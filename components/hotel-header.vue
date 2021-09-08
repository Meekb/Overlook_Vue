<template>
  <div class="header-container">
    <header class="hotel-header">
      <div class="logout">
        <button v-if="this.isValidated" @click="this.logoutUser" class="logout-btn">Logout</button>
      </div>
      <h1 class="overlook">Overlook Hotel</h1>
      <h2 class="overlook">Exclusive! Luxurious! Life-changing. . .</h2>
    </header>
  </div>
</template>

<script>
export default {
  props: [ 'logoutUser', 'isValidated' ],
}
</script>

<style>
.header-container {
  background-color: bisque;
}
.overlook {
  text-align: center;
}
.logout-btn {
  font-size: 22px;
  border-radius: 1rem;
  margin-top: 15px;
  margin-left: 143px;
}
</style>