<template>
    <v-form
      ref="LoginForm"
      :lazy-validation="lazy"
    >
      <v-text-field
        v-model="username"
        :counter="10"
        :rules="usernameRules"
        label="Username"
        required
      ></v-text-field>

      <v-text-field
        v-model="password"
        :rules="passwordRules"
        label="Password"
        required
      ></v-text-field>

 <v-btn
        color="success"
        class="mr-4"
        @click="login()"
      >
        Login
      </v-btn>

    </v-form>
  </template>
<script>
export default {
  data: () => ({
    lazy: false,
    username: '',
    usernameRules: [
      v => !!v || 'username is required'
    ],
    password: '',
    passwordRules: [
      v => !!v || 'password is required'
    ]
  }),

  methods: {
    login () {
      if (this.$refs.LoginForm.validate(true)) {
        localStorage.setItem('username', this.username)
        this.$EventBus.$emit('getUsername')
        this.$EventBus.$emit('checkLogin')
        this.$router.push({ path: '/' }).catch(() => {})
      }
    }
  }
}
</script>
