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
      this.setDetailsOfDetail()
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
    setDetailsOfDetail () {
      this.userHistory.map(item => {
        item.roomType = item.details[0].roomType
        item.bidet = item.details[0].bidet
        item.numBeds = item.details[0].numBeds
        item.bedSize = item.details[0].bedSize
      })
    },
    setTotalSpent () {
      const itemTotal = this.userHistory.map(item => 
      item.total = item.details[0].costPerNight)
      this.user.totalSpent = (this.userHistory.reduce((acc, cur) => {
        return acc += cur.total
      }, 0))
      this.user.totalSpent = this.user.totalSpent.toFixed(2)
      return this.user.totalSpent
    },
  }
}
</script>

<style>

</style>
