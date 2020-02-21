<template>
  <v-container grid-list-xs>
    <v-row wrap>
      <v-col v-for="(upcoming, index) in Upcoming" :key="index" cols="6" sm="3" md="4" lg="2">
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
      </v-col>
    </v-row>
    <!-- <div class="text-center mt-2">
      <v-pagination v-model="page" circle></v-pagination>
    </div>-->
  </v-container>
</template>

<script>
export default {
  name: "upc",
  data() {
    return {
      page: 1,
      Upcoming: []
    };
  },
  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/upcoming?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US&page=10`
      );
      let res = await req.data;
      console.log(res.results);
      this.Upcoming = res.results;
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
</style>