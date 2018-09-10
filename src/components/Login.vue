<template>
  <v-form v-model="valid" ref="form" lazy-validation>
    <v-text-field
      label="Email"
      v-model="email"
      :rules="emailRules"
      required></v-text-field>
    <v-text-field
      label="password"
      v-model="password"
      required></v-text-field>
      <v-btn
        @click="submit"
        :disabled="!valid">Submit</v-btn>
      <v-btn
        @click="clear">Clear</v-btn>
  </v-form>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    valid: true,
    email: '',
    password: '',
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /\S+@\S+\.\S+/.test(v) || 'E-mail must be valid',
    ],
  }),
  methods: {
    async submit() {
      return axios.post('http://localhost:8081/user/login',
        {
          header: {
            'Content-Type': 'application/json',
          },
        },
      ).then((response) => {
        window.localStorage.setItem('auth', response.data.token);
        this.$swal('Great', 'You are ready to start!', 'success');
      });
    },
  },
};
</script>

<style>

</style>
