<template>
  <div class="app-container">
    <hotel-header 
      :logoutUser="this.logoutUser" 
      :isValidated="this.isValidated"
    />
    <main class="main-container">
      <div 
        v-if="!isValidated" 
        class="logged-out-main"
      >
        <login-user 
          @is-validated="asyncData" 
        />
      </div>
      <div 
        v-if="isValidated" 
        class="logged-in-main"
      >
        <user-sidebar 
          :name="user.name" 
          :userHistory="userHistory" 
          :rooms="this.allRooms" 
          :userTotal="this.user.totalSpent" 
        />
        <div class="content-container">
        <booking-form 
          :user="this.user" 
          :bookings="this.allBookings" 
          :rooms="this.allRooms"
          @rooms-avail="displayAvailRooms" 
        />
        <search-results 
          v-if="this.search" 
          :availability="this.availability" 
        />
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import BookingForm from '~/components/booking-form.vue'
import SearchResults from '~/components/search-results.vue'
import userSidebar from '~/components/user-sidebar.vue'

export default {
  components: { userSidebar, BookingForm, SearchResults },
  data () {
    return {
      user: undefined,
      userHistory: undefined,
      isValidated: false,
      allBookings: undefined,
      allRooms: undefined,
      availability: undefined,
      search: false,
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

      this.setRoomTypes()
      this.setBookingDates()
      this.setBookingRooms()
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
      this.userHistory.map(item => 
      item.total = item.details[0].costPerNight)
      this.user.totalSpent = (this.userHistory.reduce((acc, cur) => {
        return acc += cur.total
      }, 0))
      this.user.totalSpent = this.user.totalSpent.toFixed(2)
      return this.user.totalSpent
    },
    setRoomTypes () {
      const formattedRooms = this.allRooms.map(rm => {
        if (rm.roomType === 'residential suite') {
          rm.roomType = 'Residential Suite'
        }
        if (rm.roomType === 'junior suite') {
          rm.roomType = 'Junior Suite'
        }
        if (rm.roomType === 'single room') {
          rm.roomType = 'Single Room'
        }
        if (rm.roomType === 'suite') {
          rm.roomType = 'Suite'
        }
        // rm.costPerNight = rm.costPerNight.toFixed(2)  
      })
      return formattedRooms
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
    setBookingRooms () {
      const formattedRoomTypes = this.allBookings.map(booking => {
        const match = this.allRooms.find(rm => rm.number === booking.roomNumber)
        return booking.roomType = match.roomType
      })
      return formattedRoomTypes
    },
    logoutUser () {
      this.allBookings = undefined
      this.allRooms = undefined
      this.availability = undefined
      this.isValidated = false
      this.search = false
      this.user = undefined
      this.userHistory = undefined
    },
    displayAvailRooms(payload) {
      this.availability = payload.availability
      this.search = payload.search
    }
  }
}
</script>

<style>
/* * {
  border: 1px solid green;
} */
.app-container {
  background-color: bisque;
  height: 95vh;
  margin: 0;
  padding: 0;
}
.logged-in-main {
  display: flex;
  justify-content: space-between;
  height: 76.5vh;
  background-color: bisque;
}
.content-container {
  width: 70%;
  background-color: bisque;
}
</style>
