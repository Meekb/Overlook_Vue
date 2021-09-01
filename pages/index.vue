<template>
  <div class="app-container">
    <hotel-header :logoutUser="this.logoutUser" :isValidated="this.isValidated"/>
    <main class="main-container">
      <div v-if="!isValidated" class="logged-out-main">
        <login-user @is-validated="asyncData" />
      </div>
      <div v-if="isValidated" class="logged-in-main">
        <user-sidebar :name="user.name" :userHistory="userHistory" :userTotal="this.user.totalSpent" />
        <div class="content-container">
        <booking-form :user="this.user" :bookings="this.allBookings" />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import BookingForm from '~/components/booking-form.vue'
import userSidebar from '~/components/user-sidebar.vue'
export default {
  components: { userSidebar, BookingForm },
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
      this.setBookingDates()
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
    formatDate (date) {
      date = date.split('/')
      const month = date[1]
      const day = date[2]
      const year = date[0]
      const formattedDate = `${month}/${day}/${year}`
      return formattedDate
    },
    setBookingDates () {
      const formattedBookings = this.allBookings.map(booking => {
        const bookingDate = booking.date
        return booking.date = this.formatDate(bookingDate)
      })
      return formattedBookings
    },
    logoutUser () {
      this.isValidated = false
      this.user = undefined
      this.userHistory = undefined
      this.allBookings = undefined
      this.allRooms = undefined
    },
  }
}
</script>

<style>
/* * {
  border: 2px solid green;
} */
.logged-in-main {
  display: flex;
  justify-content: space-between;
  /* align-items: center; */
}
.content-container {
  width: 70%;
  /* vh here for desktop skeleton */
  height: 81vh;
}
</style>
