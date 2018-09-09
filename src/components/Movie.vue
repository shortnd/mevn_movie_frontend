<template>
  <v-layout row nowrap>
    <v-flex xs4>
      <v-card>
        <v-card-title primary-title>
          <div>
            <div class="headline">{{ movie.name }}</div>
            <span class="grey--text">{{ movie.release_year }} &bull; {{ movie.genre }}</span>
          </div>
        </v-card-title>
        <h6 @click="rate" class="card-title" v-if="current_user">Rate this movie</h6>
        <v-card-text>
          {{ movie.description }}
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios';
import Vue from 'vue';
import StarRating from 'vue-star-rating';

const wrapper = document.createElement('div');
// share state
const state = {
  note: 0,
};

const RatingComponent = Vue.extend({
  data() {
    return {
      rating: 0,
    };
  },
  watch: {
    rating(newVal) {
      state.note = newVal;
    },
  },
  template: `
  <div class="rating">
  How was your experience getting help with this issues?
  <star-rating v-model="rating" :show-rating="false"></star-rating>
  </div>`,
  components: { 'star-rating': StarRating },
});

const component = new RatingComponent().$mount(wrapper);

export default {
  name: 'Movie',
  data() {
    return {
      movie: [],
    };
  },
  mounted() {
    this.fetchMovie();
  },
  methods: {
    async rate() {
      this.$swal({
        content: component.$el,
        buttons: {
          confirm: {
            value: 0,
          },
        },
      }).then(() => {
        const movieId = this.$route.params.id;
        return axios.post(`http://localhost:8081/movies/rate/${movieId}`,
          {
            header: { 'Content-Type': 'application/json' },
          },
          {
            data: { rate: state.note },
          })
          .then(() => {
            this.$swal(`Thank you for rating! ${state.note}`, 'success');
          })
          .catch((error) => {
            const message = error.response.data.message;
            this.$swal('Oh no!', `${message}`, 'error');
          });
      });
    },
    async fetchMovie() {
      return axios.get(`http://localhost:8081/movies/${this.$route.params.id}`).then((res) => { this.movie = res.data; }).catch(() => {});
    },
  },
};
</script>

<style>

</style>
