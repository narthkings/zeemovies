<template>
  <v-container>
    <div class="display-1 white--text font-weight-bold my-font">
      Toprated Movies
      <v-btn to="/rated" text dark fab small>
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>

      <v-row wrap>
        <v-col v-for="(ratedMovie, index) in TopRated" :key="index" cols="6" sm="3" md="4" lg="2">
          <!-- <v-card flat tile class="d-flex"> -->
          <v-hover v-slot:default="{ hover }">
            <v-img
              :aspect-ratio="1"
              :src="`http://image.tmdb.org/t/p/w342/${ratedMovie.poster_path}`"
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
                    <div class="body-2 mb-2 font-weight-bold title my-text">{{ ratedMovie.title }}</div>

                    <div class="mt-1 mb-4 d-flex justify-space-between">
                      <span class="mr-2 my-text">{{ ratedMovie.release_date }}</span>
                      <v-avatar
                        class="white--text caption my-text"
                        small
                        size="30"
                        color="indigo"
                      >{{ ratedMovie.vote_average }}</v-avatar>
                    </div>

                    <div class="caption text-overview my-text">{{ ratedMovie.overview }}</div>
                    <v-btn :to="`/movies/${ratedMovie.id}`" small color="success">
                      <span class="mr-1 body-2 my-text">Details</span>
                      <v-icon small>mdi-eye</v-icon>
                    </v-btn>
                  </div>
                </div>
              </v-expand-transition>
            </v-img>
          </v-hover>
          <!-- </v-card> -->
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>
<script>
export default {
  name: "ratedComponent",
  data() {
    return {
      TopRated: []
    };
  },
  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/top_rated?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US&page=1`
      );
      let res = await req.data.results;
      console.log(res);
      this.TopRated = res;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
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