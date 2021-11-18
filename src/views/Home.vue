<template>
  <div class="movie"></div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      movie: {},
      searchWords: "",
      formattedSearchArray: [],
      formattedSearchWords: "",
      randomMovieData: {},
    };
  },
  created: function () {},
  mounted: function () {},
  methods: {
    showMovieAndCreateMovie: function () {
      var splitSearchWords = this.searchWords.toLowerCase().split("");

      splitSearchWords.forEach((letter) => {
        if (letter === " ") {
          letter = "+";
          this.formattedSearchArray.push(letter);
        } else {
          this.formattedSearchArray.push(letter);
        }
      });

      this.formattedSearchWords = this.formattedSearchArray.join("");

      axios
        .get(`/movies/${this.formattedSearchWords}`)
        .then((response) => {
          console.log("Show movie", response.data);
          this.movie = response.data;
          this.searchWords = "";
          this.formattedSearchArray = [];

          var newMovieParams = {
            title: this.movie.Title,
            year: this.movie.Year,
            rated: this.movie.Rated,
            released: this.movie.Released,
            runtime: this.movie.Runtime,
            formatted_runtime: this.movie.formatted_runtime,
            actors: this.movie.Actors,
            awards: this.movie.Awards,
            box_office: this.movie.BoxOffice,
            director: this.movie.Director,
            genre: this.movie.Genre,
            language: this.movie.Language,
            plot: this.movie.Plot,
            poster: this.movie.Poster,
            entertainment_type: this.movie.Type,
            writer: this.movie.Writer,
            imdb_id: this.movie.imdbID,
            imdb_rating: this.movie.imdbRating,
            imdb_votes: this.movie.imdbVotes,
            internet_movie_database_rating: this.movie.Ratings[0].Value,
            rotten_tomatoes_rating: this.movie.Ratings[1].Value,
            metacritic_rating: this.movie.Ratings[2].Value,
          };
          axios
            .post("/movies", newMovieParams)
            .then((response) => {
              console.log("Create movie", response.data);
            })
            .catch((error) => {
              console.log("Create movie error", error.response);
              this.errors = error.response.data.errors;
            });
        })
        .catch((error) => {
          console.log("Show movie error", error.response);
          this.errors = error.response.data.errors;
        });
      this.randomMovieData = {};
    },
  },
};
</script>
