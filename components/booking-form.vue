<template>
  <div class="booking-container">
    <h3 class="user-welcome">
      Welcome back, {{ user.name.split(' ')[0] }}! Time to book your next stay:
    </h3>
    <div class="date-room-type">
      <input 
        v-model="checkinDate" 
        type="date" 
        class="calendar" 
        required
      />
      <div class="dropdown-container">
        <label class="select-text">
         Select Room Type:
          <select 
            v-model="roomType" 
            class="dropdown" 
            required
          >
            <option name="default none"></option>
            <option name="Residential Suite">Residential Suite</option>
            <option name="Suite">Suite</option>
            <option name="Junior Suite">Junior Suite</option>
            <option name="SingleRoom">Single Room</option>
          </select>
        </label>
      </div>
    </div>
    <div class="reservation info">
      <p v-if="checkinDate">Check-in: {{ checkinDate }}</p>
      <p v-if="roomType">Room Type: {{ roomType }}</p>
      <button 
        v-if="roomType" 
        @click="checkAvailability" 
        class="check-btn"
      >
      Check Availability
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['user', 'bookings', 'rooms'],
  data () {
    return {
      checkinDate: '',
      roomType: undefined,
      availRooms: [],
      queryMatch: [],
      searching: false,
    }
  },
  methods: {
    formatCheckin () {
      const dateToFormat = this.checkinDate.split('-')
      const month = dateToFormat[1]
      const day = dateToFormat[2]
      const year = dateToFormat[0]
      const formattedCheckin = `${month}/${day}/${year}`
      return formattedCheckin
    },
    checkAvailability () {
      this.availRooms = []   // reset this.availRooms to clear any prev results
      this.queryMatch = []   // reset this.queryMatch to clear any prev results
      this.searching = true
      this.checkinDate = this.formatCheckin()
      const bookedRooms = this.bookings.filter(bk => bk.date === this.checkinDate)
      this.rooms.map(rm => {
        const checker = bookedRooms.find(br => br.roomNumber === rm.number)
        if (!checker) {
          this.availRooms.push(rm)
        }
      })
      this.availRooms.forEach(rm => {
        if (rm.roomType === this.roomType) {
          this.queryMatch.push(rm)
        }
      })
      const availability = this.queryMatch
      return this.$emit('rooms-avail', {availability, search: this.searching})
    },
  },
}
</script>

<style>
p {
  font-size: 18px;
}
.booking-container {
  height: 40vh;
  background-color: bisque;
}
.user-welcome {
  text-align: left;
}
.calendar {
  font-size: 18px;
  width: 200px;
}
.date-room-type {
  display: flex;
  flex-direction: column;
}
.dropdown-container {
  display: flex;
  margin-top: 25px;
}
.select-text {
  font-size: 18px;
  font-weight: bold;
}
.dropdown {
  font-size: 18px;
}
.check-btn {
  font-size: 18px;
  border-radius: 1rem;
}
</style>