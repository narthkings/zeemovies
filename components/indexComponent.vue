<template>
  <section class="blue-grey darken-4">
    <v-carousel hide-delimiters cycle>
      <v-carousel-item
        v-for="(movie,i) in Upcoming"
        :key="i"
        :src="`http://image.tmdb.org/t/p/original/${movie.backdrop_path}`"
        :lazy-src="`http://image.tmdb.org/t/p/w92/${movie.backdrop_path}`"
      ></v-carousel-item>
    </v-carousel>

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
      <div class="display-1 white--text font-weight-bold my-font">
        Upcoming Movies
        <v-btn n-link to="/upcoming" text dark fab small>
          <v-icon>mdi-chevron-right</v-icon>
        </v-btn>

        <v-row wrap>
          <v-col v-for="(upcoming, index) in Upcoming" :key="index" cols="6" sm="3" md="4" lg="2">
            <!-- <v-card flat tile class="d-flex"> -->
            <v-hover v-slot:default="{ hover }">
              <v-img
                :aspect-ratio="1"
                :src="`http://image.tmdb.org/t/p/w342/${upcoming.poster_path}`"
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
                      <div class="body-2 mb-2 font-weight-bold title my-text">{{ upcoming.title }}</div>

                      <div class="mt-1 mb-4 d-flex justify-space-between">
                        <span class="mr-2 my-text">{{ upcoming.release_date }}</span>
                        <v-avatar
                          class="white--text caption my-text"
                          small
                          size="30"
                          color="indigo"
                        >{{ upcoming.vote_average }}</v-avatar>
                      </div>

                      <div class="caption text-overview my-text">{{ upcoming.overview }}</div>
                      <v-btn :to="`/movies/${upcoming.id}`" small color="success">
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
  </section>
</template>
<script>
export default {
  name: "indexComponent",
  data() {
    return {
      TopRated: [],
      Upcoming: []
    };
  },

  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/top_rated?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US&page=1`
      );
      let res = await req.data.results;
      console.log(res);
      this.TopRated = res.splice(0, 12);
    } catch (error) {
      console.log(error);
    }
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/upcoming?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US&page=1`
      );
      let res = await req.data.results;
      console.log(res);
      this.Upcoming = res.splice(0, 12);
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