<template>
  <div class="container">
    <h1>{{ msg }}</h1>
    <div class="input-group input-group-lg my-5">
      <span class="input-group-text" id="inputGroup-sizing-lg">Search</span>
      <input
        type="text"
        class="form-control"
        aria-label="Sizing example input"
        aria-describedby="inputGroup-sizing-lg"
        v-model="word"
        placeholder="what's your favorite movie?"
      />
    </div>
    <div v-if="movies" class="container">
      <div v-for="(movie, index) of movies" :key="index">
        <div class="card bg-light">
          <div class="card-body row">
            <div class="col">
              {{ movie.release_date }}
            </div>
            <div class="col">
              {{ movie.original_title }}
            </div>
            <div class="col">
              {{ movie.overview }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import { mapState } from "vuex";
import { MyType } from "../types/my-type";
import { getMovies } from "../services/movie-service";
import { Movie } from "../types/page-movies";

@Options({
  props: {
    msg: String,
  },
  computed: mapState(["message"]),
  methods: {
    getMovies,
  },
  watch: {
    async word(val: string) {
      await getMovies(val).then((res) => {
        this.movies = res.results;
      });
    },
  },
})
export default class Movies extends Vue {
  msg!: string;
  message!: MyType;
  movies?: Array<Movie> = [];
  word?: string = "";

  async mounted(): Promise<void> {
    console.log("state -> ", this.message);
    await getMovies().then((res) => {
      this.movies = res.results;
      console.log(this.movies);
    });
  }
}
</script>

<style scoped></style>
