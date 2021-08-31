<template>
  <div class="booking-container">
    <h3 class="user-welcome">
      Welcome back, {{ user.name.split(' ')[0] }}! Time to book your next stay:
    </h3>
    <div class="date-room-type">
      <input v-model="checkinDate" type="date" class="calendar" required/>
      <div class="dropdown-container">
        <label class="select-text">
         Select Room Type:
          <select v-model="roomType" class="dropdown" required>
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
      <p v-if="checkinDate">Check-in: {{ `${checkinDate.split('-')[1]}/${checkinDate.split('-')[2]}/${checkinDate.split('-')[0]}`}}</p>
      <p v-if="roomType">Room Type: {{ roomType }}</p>
      <button v-if="roomType" class="check-btn">Check Availability</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['user'],
  data () {
    return {
      checkinDate: '',
      roomType: undefined
    }
  },
  method: {
    formatCheckin () {
      this.checkinDate.split('-')
      const month = dateToFormat[1]
      const day = dateToFormat[2]
      const year = dateToFormat[0]
      const formattedCheckin = `${month}/${day}/${year}`
      this.checkinDate = formattedCheckin
      return this.checkinDate
    }
  },
}
</script>

<style>
p {
  font-size: 18px;
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