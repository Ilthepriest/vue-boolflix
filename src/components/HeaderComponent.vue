<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light bg_header">
      <div
        class="container-fluid d-flex justify-content-around align-items-center"
      >
        <img class="logo" src="@/assets/img/logoNetflix.png" alt="" />

        <button
          class="navbar-toggler color_red"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div
          class="collapse navbar-collapse justify-content-end mt-3"
          id="navbarSupportedContent"
        >
          <form class="d-flex" @submit.prevent>
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search"
              aria-label="Search"
              v-model="inputSearch"
            />
            <button class="btn btn-outline-light" @click="search" type="submit">
              Search
            </button>
          </form>
        </div>
      </div>
    </nav>
    <div class="container-fluid">
      <div
        class="row row-cols-1 row-cols-lg-6 d-flex justify-content-center p-3"
      >
        <!-- Film and series -->
        <div
          class="
            col
            card
            d-flex
            text-center
            border_radius
            mx-3
            p-3
            gy-5
            hover_card
          "
          v-for="movie in movies"
          :key="movie.id"
        >
          <img
            class="cover h-100 img_film"
            v-if="!movie.poster_path"
            src="https://www.salonlfc.com/wp-content/uploads/2018/01/image-not-found-scaled.png"
            alt=""
          />
          <img
            class="img_film"
            :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path"
            alt=""
          />
          <div class="over_view text_justify p-4">
            <h3 class="text_red">{{ movie.title }}</h3>
            <div>{{ movie.name }}</div>
            <div>{{ movie.original_name }}</div>
            <div>{{ movie.original_title }}</div>
            <div>{{ movie.original_language }}</div>
            <lang-flag :iso="movie.original_language" />
            <div>{{ movie.vote_average }}</div>
            <div>{{ movie.overview }}</div>
            <div class="star">
              <font-awesome-icon
                class="color_star"
                icon="fa-solid fa-star"
                v-for="star in arrotondo(movie.vote_average)"
                :key="star.id"
              />
              <font-awesome-icon
                v-for="star in 5 - arrotondo(movie.vote_average)"
                :key="star.id"
                icon="fa-solid fa-star"
              />
              <!-- Sezione attori ------------------------------------------------------->
              <h5 class="text_red">CAST</h5>
              <div>
                <div v-if="movie.actors.length > 0">
                  <div v-for="actor in movie.actors" :key="actor.id">
                    <div>{{ actor.name }}</div>
                    <img
                      class="w-50"
                      :src="
                        'https://image.tmdb.org/t/p/w342' + actor.profile_path
                      "
                      alt=""
                    />
                  </div>
                </div>
                <div v-else>
                  <p>Attori non trovati...</p>
                </div>
              </div>
              <!-- Fine attori -->

              <!-- Selezione Generi -->
              <h5 class="text_red">GENERE</h5>
              <div>
                <div v-for="genere in genres" :key="genere.id">
                  <div v-if="movie.genre_ids.includes(genere.id)">
                    {{ genere.name }}
                  </div>
                </div>
              </div>
              <!-- Fine Generi -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "SiteHeader",
  data() {
    return {
      movies: [],
      error: null,
      query: "",
      inputSearch: "",
      genres: [],
    };
  },
  methods: {
    async search() {
      console.log("Searching ...");
      this.query = this.inputSearch;
      await axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=6492e216147f755d586c53abf227d9a5&language=it-IT&page=1&include_adult=false&query=${this.query}`
        )
        .then((results) => {
          this.movies = results.data.results;
        })
        .catch((error) => {
          console.log(error);
          this.error = `Scusa c'è un errore ${this.error}`;
        });

      await axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=6492e216147f755d586c53abf227d9a5&language=it-IT&page=1&include_adult=false&query=${this.query}`
        )
        .then((results) => {
          this.movies = [...this.movies, ...results.data.results];
        })
        .catch((error) => {
          console.log(error);
          this.error = `Scusa c'è un errore ${this.error}`;
        });
      this.movies.forEach((element) => {
        // console.log({ element });
        axios
          .get(
            `https://api.themoviedb.org/3/movie/${element.id}/credits?api_key=6492e216147f755d586c53abf227d9a5&language=en-US`
          )
          .then((results) => {
            // console.log(results);
            let castFive = results.data.cast.slice(0, 5);
            this.$set(element, "actors", castFive);
          })
          .catch((error) => {
            console.log(error);

            this.$set(element, "actors", []);
          });
      });

      await axios
        .get(
          "https://api.themoviedb.org/3/genre/movie/list?api_key=6492e216147f755d586c53abf227d9a5&language=en-US"
        )
        .then((results) => {
          this.genres = results.data.genres;
        })
        .catch((error) => {
          console.log(error);
          this.error = `Scusa c'è un errore ${this.error}`;
        });
      // console.log(this.movies);
    },

    arrotondo(num) {
      // console.log( Math.round(num / 2));
      return Math.round(num / 2);
    },
  },
};
</script>

<style lang="scss" scoped>
.bg_header {
  background-color: #141414 !important;
}
.color_red {
  border: 4px solid #e10228;
  border-color: red !important;
  background-color: white;
}
.logo {
  width: 100px;
}
.color_star {
  color: goldenrod;
}
.border_radius {
  border-radius: 10px;
}
.cover {
  object-fit: cover;
}
.over_view {
  display: none;
  overflow: auto;
}
.hover_card {
  background-color: #141414;
}
.hover_card:hover {
  position: relative;
  background-color: #141414;
}
.hover_card:hover .over_view {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  max-height: 90%;
  width: 98.5%;
  padding: 10px;
}
.hover_card:hover .img_film {
  opacity: 0.2;
}
.text_red {
  color: #e10228;
}
.text_justify {
  text-align: justify;
}
</style>