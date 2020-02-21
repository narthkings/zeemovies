<template>
  <section class="blue-grey darken-4">
    <section>
      <v-img
        class="bg-drop"
        :src="`http://image.tmdb.org/t/p/original/${singleFilm.backdrop_path}`"
        :lazy-src="`http://image.tmdb.org/t/p/w92/${singleFilm.backdrop_path}`"
        cover
      >
        <v-container grid-list-lg>
          <v-row>
            <v-col sm="4">
              <v-img
                class="rounded-poster"
                :lazy-src="`http://image.tmdb.org/t/p/w92/${singleFilm.poster_path}`"
                :src="`http://image.tmdb.org/t/p/w342/${singleFilm.poster_path}`"
              ></v-img>
            </v-col>
            <v-col
              class="white--text go-down"
              sm="8"
              :mt-5="this.$vuetify.breakpoint.lgAndUp"
              style="z-index:1"
            >
              <div>
                <span class="display-1 font-weight-black">{{ singleFilm.title }}</span>
                <span class="headline ml-2">({{ singleFilm.release_date }})</span>
              </div>
              <div>
                <span
                  class="mr-3 font-weight-medium"
                  v-for="(genre, id) in singleFilm.genres"
                  :key="id"
                >{{genre.name}}</span>
              </div>

              <v-row wrap class="mt-4">
                <v-col>
                  <div>
                    <v-avatar
                      class="white--text subtitle-2 my-text"
                      small
                      size="35"
                      color="indigo"
                    >{{singleFilm.vote_average}}</v-avatar>
                    <span class="font-weight-bold ml-2">Average rating</span>
                  </div>
                </v-col>
                <v-col sm="6">
                  <div class="font-weight-bold">
                    Runtime:
                    {{ singleFilm.runtime }} mins
                  </div>
                </v-col>
              </v-row>
              <div class="mt-4">
                <div class="font-weight-bold headline">Overview</div>
                <div class="mt-2 body-2 white--text">{{singleFilm.overview}}</div>
              </div>
              <div v-if="credit" class="mt-3">
                <div class="font-weight-bold headline">Featured Crew</div>

                <v-row>
                  <v-col
                    class="mt-2"
                    cols="6"
                    sm="4"
                    lg="3"
                    v-for="(crew, id) in featuredCrew"
                    :key="id"
                  >
                    <div class="font-weight-bold body-2">{{ crew.name }}</div>
                    <div class="caption">{{ crew.job }}</div>
                  </v-col>
                </v-row>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-img>
    </section>
    <v-container grid-list-lg>
      <div class="headline font-weight-bold mt-4 mb-2 white--text">Top Billed Cast</div>
      <v-row v-if="credit != null" wrap class="mt-4">
        <v-col cols="6" sm="3" lg="2" v-for="(cast, id) in featuredCasts" :key="id">
          <v-sheet @click="getCast(cast.id)" class="rounded-profile white">
            <v-img
              v-if="cast.profile_path != null"
              max-height="220"
              class="profile-image"
              :src="`http://image.tmdb.org/t/p/w342/${cast.profile_path}`"
              :lazy-src="`http://image.tmdb.org/t/p/w92/${cast.profile_path}`"
            ></v-img>
            <v-sheet min-height="200" class="text-xs-center profile-image" v-else>
              <v-icon size="100" class="mt-5">mdi-account-circle</v-icon>
            </v-sheet>
            <v-col class="pa-3">
              <div class="font-weight-bold my-text">{{ cast.name }}</div>
              <div class="caption mt-2">{{ cast.character }}</div>
            </v-col>
          </v-sheet>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>
<script>
export default {
  name: "singleMovie",
  data() {
    return {
      singleFilm: "",
      credit: null
    };
  },
  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US`
      );
      let res = await req.data;
      this.singleFilm = res;
      console.log(res);
    } catch (error) {
      console.log(error);
    }

    try {
      let request = await this.$axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}/credits?api_key=62722bcd117e9d14d0bed44381ccf36f`
      );
      let response = await request.data;
      this.credit = response;
      //   console.log(response);
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    getCast(castID) {
      this.$router.push(`/cast/${castID}`);
    }
  },
  computed: {
    featuredCrew() {
      return this.credit.crew.splice(0, 6);
    },
    featuredCasts() {
      return this.credit.cast.splice(0, 6);
    }
  }
};
</script>
<style scoped>
/* @import url("https://fonts.googleapis.com/css?family=Fondamento|Odibee+Sans|Russo+One|Sriracha&display=swap"); */
.bg-drop::before {
  content: "";
  position: absolute;
  transform: translate(0, 0);
  background: rgba(166, 19, 224, 0.829);
  z-index: 0;
  width: 100%;
  height: 100%;
}
.rounded-poster {
  border-radius: 1.5rem;
}
.rounded-profile {
  cursor: pointer;
  border-radius: 20px 20px 8px 8px;
}
.profile-image {
  border-radius: 8px 8px 0 0;
}
.my-text {
  font-family: "Fondamento", cursive !important;
}
@media screen and (min-width: 1264px) {
  .go-down {
    margin-top: 4rem;
  }
}
</style>