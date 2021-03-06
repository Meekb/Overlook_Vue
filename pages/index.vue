<template>
  <div class="app-container">
    <hotel-header 
      :logoutUser="this.logoutUser" 
      :isValidated="this.isValidated"
    />
    <main class="main-container">
      <div class="image-container">
        <img v-if="!this.isValidated" src="../static/OverlookHallway.png" width="300px"/>
      </div>
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
      <div class="sidebar-container">
        <user-sidebar 
          :name="user.name" 
          :userHistory="userHistory" 
          :rooms="this.allRooms" 
          :userTotal="this.user.totalSpent" 
        />
      </div>
      <div class="content-container">
        <div class="form-and-results">
          <booking-form 
            :user="this.user" 
            :bookings="this.allBookings" 
            :rooms="this.allRooms"
            @rooms-avail="displayAvailRooms"
            @search-date="setSearchDate" 
          />
          <search-results
            v-show="!this.reservation"
            @new-reservation="bookRoom" 
            :search="this.search" 
            :availability="this.availability" 
          />
        </div>
        <div v-if="this.reservation" class="confirmation-container">
          <p class="booking-confirmation">
            Success! You've booked Room# {{ reservation.roomNumber }} for the night of {{ reservation.date }}
          </p>
          <p class="booking-confirmation">
            Confirmation# {{ reservation.confirmation }}.
          </p>
          <p class="booking-confirmation">
            Please retain for your records.
          </p>
          <button @click="updateAndClear" class="done-btn">Done</button>
        </div>
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
  head () {
    return {
      title: 'The Overlook Hotel',
      meta: [
        {name: 'overlook:homepage'}
      ],
      link: [
        {        
          rel: 'canonical',
          href: this.$route.path
        }
      ],
    }
  },
  data () {
    return {
      user: undefined,
      userHistory: undefined,
      isValidated: false,
      allBookings: undefined,
      allRooms: undefined,
      availability: undefined,
      search: false,
      searchdate: undefined,
      reservation: undefined,
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
    async bookRoom(payload) {
      const newReservation = payload.newReservation
      newReservation.userID = this.user.id
      newReservation.date = this.searchDate
      const confirmed = await this.$axios.$post('http://localhost:3001/api/v1/bookings', newReservation)
      newReservation.confirmation = confirmed.newBooking.id
      this.reservation = newReservation
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
      this.reservation = undefined
    },
    displayAvailRooms(payload) {
      this.availability = payload.availability
      this.search = payload.search
    },
    setSearchDate(payload) {
      this.searchDate = payload
      this.searchDate = this.searchDate.split('-').join('/')
      return this.searchDate
    },
    updateAndClear() {
      // this method needs to trigger other components -booking form- to clear data
      this.availability = undefined
      this.search = false
      this.searchDate = undefined
      this.reservation = undefined
      this.asyncData()
    },
  }
}
</script>

<style>
/* * {
  border: 1px solid green;
} */
.app-container {
  display: flex;
  flex-direction: column;
  background-color: #c1ac93;
  color: black;
  height: 98vh;
  margin: 0;
  padding: 0;
}
.logged-in-main {
  display: flex;
  justify-content: center;
  align-items: center;
}
.logged-out-main {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 148px;
}
.main-container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  background-color: #c1ac93;
  margin-top: 25px;
}
.form-and-results {
  display: flex;
  flex-direction: column;
}
.content-container {
  margin-left: 40px;
}
.sidebar-container {
  width: 325px;
  height: 74vh;
  overflow: scroll;
  text-align: center;
  background-color: #82724a;
  border: 4px solid #eaa654;
  border-radius: 1.5rem;
  margin-left: 25px;
}
.confirmation-container {
  height: 27.5vh;
  text-align: center;
  background-color: #82724a;
  padding: 25px;
  width: 920px;
  border-radius: 1.5rem;
  border: 5px solid #eaa654;
}
.booking-confirmation {
  font-size: 25px;
  font-weight: bold;
}
.done-btn {
  font-size: 18px;
  font-weight: bold;
  border-radius: 1rem;
}
img {
  border-radius: 1.5rem;
  border: 5px solid #82724a;
  margin-left: 55px;
  margin-top: 3 5px;
}
</style>
