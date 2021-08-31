<template>
  <div class="main-container">
    <hotel-header />
    <login-user @is-validated="asyncData" />
    <user-sidebar v-if="isValidated" :name="user.name" :userHistory="userHistory" :userTotal="this.user.totalSpent" />
  </div>
</template>

<script>
import userSidebar from '~/components/user-sidebar.vue'
export default {
  components: { userSidebar },
  data () {
    return {
      user: undefined,
      userHistory: undefined,
      isValidated: false,
      allBookings: undefined,
      allRooms: undefined,
    }
  },
  methods: {
    async asyncData(payload, $axios) {
      const profile = await this.$axios.$get(`http://localhost:3001/api/v1/customers/${payload.userId}`)
      this.user = profile
      this.isValidated = true

      const bookings = await this.$axios.$get('http://localhost:3001/api/v1/bookings')
      this.allBookings = bookings.bookings

      const rooms = await this.$axios.$get('http://localhost:3001/api/v1/rooms')
      this.allRooms = rooms.rooms

      this.setUserHistory()
      this.setTotalSpent()
    },
    setUserHistory () {
      const userHistory = this.allBookings.filter(booking => booking.userID === this.user.id)
      this.userHistory = userHistory

      const details = this.userHistory.map(item => {
        item.details = item;
        const detail = this.allRooms.filter(room => {
          return room.number === item.roomNumber
        })
        item.details = [...detail]
      })
      return details
    },
    setTotalSpent () {
      const itemTotal = this.userHistory.map(item => 
      item.total = item.details[0].costPerNight)
      this.user.totalSpent = (this.userHistory.reduce((acc, cur) => {
        return acc += cur.total
      }, 0))
      console.log(this.user)
    },
  }
}
</script>

<style>

</style>
