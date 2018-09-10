<template>
  <v-form v-model="valid" ref="form" lazy-validation>
    <v-text-field
      label="Name"
      v-model="name"
      required></v-text-field>
    <v-text-field
      label="Email"
      v-model="email"
      :rules="emailRules"
      required></v-text-field>
    <v-text-field
      name="Password"
      v-model="password"
      label="Password"
      required></v-text-field>
    <v-text-field
      name="input-7-1"
      label="Confirm Password"
      v-model="confirm_password"></v-text-field>
    <v-btn
      @click="submit"
      :disabled="!valid">Submit</v-btn>
    <v-btn @click="clear">Clear</v-btn>
  </v-form>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    valid: true,
    name: '',
    email: '',
    password: '',
    confirm_password: '',
    emailRules: [
      v => !!v || 'Email is required',
      v => /\S+@\S+\.\S+/.test(v) || 'Email must be valid',
    ],
  }),
  methods: {
    async submit() {
      if (this.$refs.form.validate()) {
        return axios.post('http://localhost:8081/users/register',
          {
            header: {
              'Content-Type': 'application/json',
            },
          },
          {
            data: {
              name: this.name,
              email: this.email,
              password: this.password,
            },
          },
        ).then(() => {
          this.$swal(
            'Great',
            'You have been successfully registered!',
            'success',
          );
          this.$router.push({ name: 'Login' });
        }).catch((error) => {
          const message = error.response.data.message;
          this.$swal('Oh no!', `${message}`, 'error');
        });
      }
      return true;
    },
    clear() {
      this.$refs.form.reset();
    },
  },
};
</script>

<style>

</style>
