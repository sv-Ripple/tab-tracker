<template>
  <v-layout column>
    <v-flex xs5 offset-xs1>
      <panel title="Register">
        <form>
         <v-text-field
            label="email"
            v-model="email"
          ></v-text-field>
        <v-text-field
            label="password"
            type="password"
            v-model="password"
            autocomplete="new-password"
          ></v-text-field>
        <div class="danger-alert" v-html="error" />
        <v-btn class="cyan" dark @click="register">Register</v-btn>
        </form>
      </panel>
    </v-flex>
  </v-layout>
</template>

<script>
import AuthenticationService from '@/services/AuthenticationService'

export default {
  data () {
    return {
      email: '',
      password: '',
      error: null
    }
  },
  methods: {
    async register () {
      try {
        const response = await AuthenticationService.register({
          email: this.email,
          password: this.password
        })
        this.$store.dispatch('setToken', response.data.token)
        this.$store.dispatch('setUser', response.data.user)
        this.$router.push({
          name: 'songs'
        })
      } catch (error) {
        this.error = error.response.data.error
      }
    }
  }
}
</script>

<style scoped>
.tbox {
  border: 1px solid black;
}
.headline {
  position: justify;
  padding-left: 110px;
}
</style>
