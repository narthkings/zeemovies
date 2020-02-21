<template>
  <v-container>
    <div v-if="noData">
      <v-container>
        <div class="justify-center align-center">
          <h2>Movie with title "{{this.$route.params.id}}" cannot be found</h2>
        </div>
      </v-container>
    </div>

    <v-row wrap>
      <v-col v-for="(search, index) in searchedMovies" :key="index" cols="6" sm="3" md="4" lg="2">
        <!-- <v-card flat tile class="d-flex"> -->
        <div v-if="search.poster_path !==null">
          <v-hover v-slot:default="{ hover }">
            <v-img
              :aspect-ratio="1"
              :src="`http://image.tmdb.org/t/p/w342/${search.poster_path}`"
              class="round-img grey lighten-2"
              height="270px"
            >
              <template v-slot:placeholder>
                <v-row class="fill-height ma-0" align="center" justify="center">
                  <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                </v-row>
              </template>

              <v-expand-transition>
                <div
                  v-if="hover"
                  class="d-flex transition-fast-in-fast-out black darken-3 v-card--reveal subtitle-1 white--text py-4 px-2"
                  style="height: 100%;"
                >
                  <div class="text-center slide-over">
                    <div class="body-2 mb-2 font-weight-bold title my-text">{{ search.title }}</div>

                    <div class="mt-1 mb-4 d-flex justify-space-between">
                      <span class="mr-2 my-text">{{ search.release_date }}</span>
                      <v-avatar
                        class="white--text caption my-text"
                        small
                        size="30"
                        color="indigo"
                      >{{ search.vote_average }}</v-avatar>
                    </div>

                    <div class="caption text-overview my-text">{{ search.overview }}</div>
                    <v-btn :to="`/movies/${search.id}`" small color="success">
                      <span class="mr-1 body-2 my-text">Details</span>
                      <v-icon small>mdi-eye</v-icon>
                    </v-btn>
                  </div>
                </div>
              </v-expand-transition>
            </v-img>
          </v-hover>
        </div>
        <div v-else>
          <v-hover v-slot:default="{ hover }">
            <!-- <v-sheet> -->
            <v-img
              :aspect-ratio="1"
              :src="`http://image.tmdb.org/t/p/w342/${search.poster_path}`"
              class="round-img grey lighten-2"
              height="270px"
            >
              <!-- <template v-slot:placeholder>
                <v-row class="fill-height ma-0" align="center" justify="center">
                  <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                </v-row>
              </template>-->
              <div class="d-flex justify-center center">No image</div>
              <v-expand-transition>
                <div
                  v-if="hover"
                  class="d-flex transition-fast-in-fast-out black darken-3 v-card--reveal subtitle-1 white--text py-4 px-2"
                  style="height: 100%;"
                >
                  <div class="text-center slide-over">
                    <div class="body-2 mb-2 font-weight-bold title my-text">{{ search.title }}</div>

                    <div class="mt-1 mb-4 d-flex justify-space-between">
                      <span class="mr-2 my-text">{{ search.release_date }}</span>
                      <v-avatar
                        class="white--text caption my-text"
                        small
                        size="30"
                        color="indigo"
                      >{{ search.vote_average }}</v-avatar>
                    </div>

                    <div class="caption text-overview my-text">{{ search.overview }}</div>
                    <v-btn :to="`/movies/${search.id}`" small color="success">
                      <span class="mr-1 body-2 my-text">Details</span>
                      <v-icon small>mdi-eye</v-icon>
                    </v-btn>
                  </div>
                </div>
              </v-expand-transition>
              <!-- </v-sheet> -->
            </v-img>
          </v-hover>
        </div>
      </v-col>
    </v-row>
    <!-- </div> -->
  </v-container>
</template>
<script>
export default {
  name: "index",
  data() {
    return {
      searchedMovies: [],
      noData: false
    };
  },

  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US&query=${this.$route.params.id}&page=1`
      );
      if (req.data.total_results > 0) {
        this.searchedMovies = await req.data.results;
        console.log(req.data.results);
        this.noData = false;
      } else {
        this.noData = true;
      }
    } catch (error) {
      console.log(error);
    }
  }
};
</script>
<style scoped>
.poster {
  border-radius: 8px;
}
.center {
  margin-top: 7rem;
}
/* .v-card--reveal-2 {
  align-items: center;
  top: 50%;
  text-align: center;
  justify-content: center;
  position: absolute;
  width: 100%;
} */
.my-text {
  font-family: "Sriracha", cursive !important;
}
.text-overview {
  text-overflow: ellipsis;
  display: inline-block;
  overflow: hidden;
  width: 100%;
  max-height: 35%;
  margin-bottom: 10px;
}
.my-font {
  font-family: "Odibee Sans", cursive !important;
}
.round-img {
  border-radius: 0.3rem;
}
.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: 0.9;
  position: absolute;
  width: 100%;
}
.slide-over {
  height: 100%;
}
</style>