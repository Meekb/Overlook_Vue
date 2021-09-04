<template>
  <div v-if="!this.validated" class="login-container">
    <div class="please-login">
      <p>
      Please login to continue.
      </p>
    <p>Not a member yet? Accepting new members this Fall!</p>
  </div>
    <div class="username-container">
      <label>Username:</label>
      <input 
        class="login-input username" 
        v-model="username" 
        type="text" 
        required/>
    </div>
    <div class="password-container">
      <label>Password:</label>
      <input 
        class="login-input password" 
        v-model="password" 
        type="password" 
        required/>
    </div>
    <button @click="validateLogin" class="login-btn">Login</button>
    <error-login v-if="this.error"/>
  </div>
</template>

<script>
export default {
  data () {
    return {
      username: '',
      password: '',
      error: false,
      validated: false
    }
  },
  methods: {
    errorTimeout () {
      this.username = ''
      this.password = ''
      this.error = false
    },
    validateLogin () {
      const setup = this.username.split('r')
      const userError1 = this.username.length < 9
      const userError2 = this.username.length > 10
      const userError3 = (setup[0] !== 'custome')
      const userError4 = (Number(setup[1]) < 1)
      const userError5 = (Number(setup[1]) > 50)
      const passError1 = (this.password !== 'overlook2021')
      if (userError1 || userError2 || userError3 || userError4 || userError5 || passError1) {
        this.error = true
        setTimeout(this.errorTimeout, 3000)
        return
      } 
      this.validated = true;
      const userId = this.username.split('r')[1]
      this.$emit('is-validated', {userId})
    },
  }
}
</script>

<style>
label {
  font-size: 18px;
  margin-right: 6px;
}
.login-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0;
}
.please-login {
  font-size: 18px;
  text-align: center;
}
.login-input {
  font-size: 18px;
  width: 150px;
}
.login-btn {
  font-size: 18px;
  margin-top: 20px;
  border-radius: 1rem;
}
.username,
.password {
  margin-top: 15px;
}
</style>