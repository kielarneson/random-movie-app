<template>
  <div class="movie">
    <form @submit.prevent="showMovieAndCreateMovie()">
      <input class="search" type="text" v-model="searchWords" placeholder="search movies" />
    </form>

    <br />
    <div class="filter-by-genre">
      <input type="checkbox" id="checkbox-action" v-model="action" />
      <label for="checkbox">Action</label>
      <!-- <input type="checkbox" id="checkbox-action" v-model="biography" />
      <label for="checkbox">Biography</label> -->
      <input type="checkbox" id="checkbox-comedy" v-model="comedy" />
      <label for="checkbox">Comedy</label>
      <!-- <input type="checkbox" id="checkbox-crime" v-model="crime" />
      <label for="checkbox">Crime</label> -->
      <input type="checkbox" id="checkbox-documentary" v-model="documentary" />
      <label for="checkbox">Documentary</label>
      <input type="checkbox" id="checkbox-drama" v-model="drama" />
      <label for="checkbox">Drama</label>
      <input type="checkbox" id="checkbox-family" v-model="family" />
      <label for="checkbox">Family</label>
      <input type="checkbox" id="checkbox-horror" v-model="horror" />
      <label for="checkbox">Horror</label>
      <input type="checkbox" id="checkbox-romance" v-model="romance" />
      <label for="checkbox">Romance</label>
      <input type="checkbox" id="checkbox-scifi" v-model="sciFi" />
      <label for="checkbox">Sci-Fi</label>
      <input type="checkbox" id="checkbox-sports" v-model="sport" />
      <label for="checkbox">Sports</label>
      <input type="checkbox" id="checkbox-thriller" v-model="thriller" />
      <label for="checkbox">Thriller</label>
    </div>

    <div class="movie-show">
      <button class="random-movie" @click="randomMovie()">Generate Random Movie</button>
      <div v-if="Object.keys(movie).length !== 0">
        <h1>{{ movie.Title }}</h1>
        <h3>Runtime: {{ movie.formatted_runtime }} | Year: {{ movie.Year }}</h3>
        <h3>Director: {{ movie.Director }}</h3>
        <h3>Actors: {{ movie.Actors }}</h3>
        <img :src="`${movie.Poster}`" alt="" />
        <h4>Plot: {{ movie.Plot }}</h4>
        <h4>{{ movie.Awards }} | Box Office: {{ movie.BoxOffice }}</h4>
        <h4>
          IMDB Rating: {{ movie.Ratings[0].Value }} | Rotten Tomatoes Rating: {{ movie.Ratings[1].Value }} | Metacritic
          Rating: {{ movie.Ratings[2].Value }}
        </h4>
      </div>

      <div v-if="Object.keys(randomMovieData).length !== 0">
        <h1>{{ randomMovieData.title }}</h1>
        <h3>Runtime: {{ randomMovieData.formatted_runtime }} | Year: {{ randomMovieData.year }}</h3>
        <h3>Director: {{ randomMovieData.director }}</h3>
        <h3>Actors: {{ randomMovieData.actors }}</h3>
        <img :src="`${randomMovieData.poster}`" alt="" />
        <h4>Plot: {{ randomMovieData.plot }}</h4>
        <h4>{{ randomMovieData.awards }} | Box Office: {{ randomMovieData.box_office }}</h4>
        <h4>
          IMDB Rating: {{ randomMovieData.internet_movie_database_rating }} | Rotten Tomatoes Rating:
          {{ randomMovieData.rotten_tomatoes_rating }} | Metacritic Rating: {{ randomMovieData.metacritic_rating }}
        </h4>
      </div>
    </div>
  </div>
</template>

<style>
.search {
  text-align: center;
}
</style>

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
      action: false,
      // biography: false,
      comedy: false,
      // crime: false,
      documentary: false,
      drama: false,
      family: false,
      horror: false,
      romance: false,
      sciFi: false,
      sport: false,
      thriller: false,
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
    randomMovie: function () {
      if (this.action === true) {
        // Nested if/else conditionals / Do this for all other possibilities
        if (this.comedy === true) {
          axios.get("/action_comedies").then((response) => {
            console.log("Random Action Comedy", response);
            this.randomMovieData = response.data;
          });
          // Add documentary here
        } else if (this.drama === true) {
          axios.get("/action_dramas").then((response) => {
            console.log("Random Action Drama", response);
            this.randomMovieData = response.data;
          });
        } else if (this.family === true) {
          axios.get("/action_families").then((response) => {
            console.log("Random Action Family", response);
            this.randomMovieData = response.data;
          });
        } else if (this.horror === true) {
          axios.get("/action_horrors").then((response) => {
            console.log("Random Action Horror", response);
            this.randomMovieData = response.data;
          });
        } else if (this.romance === true) {
          axios.get("/action_romances").then((response) => {
            console.log("Random Action Romance", response);
            this.randomMovieData = response.data;
          });
        } else {
          axios.get("/actions").then((response) => {
            console.log("Random Action", response);
            this.randomMovieData = response.data;
          });
        }
        // } else if (this.biography === true) {
        //   axios.get("/biographies").then((response) => {
        //     console.log("Random Biography", response);
        //     this.randomMovieData = response.data;
        //   });
      } else if (this.comedy === true) {
        axios.get("/comedies").then((response) => {
          console.log("Random Comedy", response);
          this.randomMovieData = response.data;
        });
        // } else if (this.crime === true) {
        //   axios.get("/crimes").then((response) => {
        //     console.log("Random Crime", response);
        //     this.randomMovieData = response.data;
        //   });
      } else if (this.documentary === true) {
        axios.get("/documentaries").then((response) => {
          console.log("Random Documentary", response);
          this.randomMovieData = response.data;
        });
      } else if (this.drama === true) {
        axios.get("/dramas").then((response) => {
          console.log("Random Drama", response);
          this.randomMovieData = response.data;
        });
      } else if (this.family === true) {
        axios.get("/families").then((response) => {
          console.log("Random Family", response);
          this.randomMovieData = response.data;
        });
      } else if (this.horror === true) {
        axios.get("/horrors").then((response) => {
          console.log("Random Horror", response);
          this.randomMovieData = response.data;
        });
      } else if (this.romance === true) {
        axios.get("/romances").then((response) => {
          console.log("Random Romance", response);
          this.randomMovieData = response.data;
        });
      } else if (this.sciFi === true) {
        axios.get("/scifis").then((response) => {
          console.log("Random Sci-Fi", response);
          this.randomMovieData = response.data;
        });
      } else if (this.sport === true) {
        axios.get("/sports").then((response) => {
          console.log("Random Sport", response);
          this.randomMovieData = response.data;
        });
      } else if (this.thriller === true) {
        axios.get("/thrillers").then((response) => {
          console.log("Random Thriller", response);
          this.randomMovieData = response.data;
        });
      } else {
        axios.get("/movies").then((response) => {
          console.log("Random movie", response);
          this.randomMovieData = response.data;
        });
      }
      this.movie = {};
    },
  },
};
</script>
