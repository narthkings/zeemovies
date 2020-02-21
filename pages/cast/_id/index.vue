<template>
  <section>
    <v-sheet class="white">
      <v-container v-if="castDetails" grid-list-lg>
        <v-row wrap>
          <v-col sm="4">
            <v-img
              class="rounded-poster mx-auto"
              v-if="castDetails.profile_path != null"
              :lazy-src="`http://image.tmdb.org/t/p/w92/${castDetails.profile_path}`"
              :src="`http://image.tmdb.org/t/p/w342/${castDetails.profile_path}`"
            ></v-img>
            <v-sheet min-height="300" class="rounded-poster text-xs-center" v-else>
              <v-icon size="100" class="mt-5">mdi-account-circle</v-icon>
            </v-sheet>
          </v-col>

          <v-col sm="8" class="go-down">
            <span class="display-1 font-weight-black mytext">{{castDetails.also_known_as[0]}}</span>
            <div class="mt-2 title font-weight-bold mytext">Biography</div>
            <div class="mt-2 font-weight-light subtitle-1 mytext">{{ castDetails.biography}}</div>

            <v-row class="mt-2" wrap>
              <v-col sm="4">
                <div>Also known as</div>
                <div
                  class="body-1 mytext"
                  v-for="(cast, i) in castDetails.also_known_as"
                  :key="i"
                >{{ cast }}</div>
              </v-col>

              <v-col sm="4">
                <div>Gender</div>
                <div class="body-1 mytext">{{ checkGender}}</div>

                <div class="mt-4">Known for</div>
                <div class="body-1 mytext">{{castDetails.known_for_department}}</div>
              </v-col>

              <v-col sm="4">
                <div>Place of birth</div>
                <div class="body-1 mytext">{{castDetails.place_of_birth}}</div>

                <div class="mt-4">Birthday</div>
                <div class="body-1 mytext">{{castDetails.birthday}}</div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-sheet>

    <v-container grid-list-xs>
      <v-row wrap>
        <v-col
          v-for="(associated, index) in associatedMovie"
          :key="index"
          cols="6"
          sm="3"
          md="4"
          lg="2"
        >
          <!-- <v-card flat tile class="d-flex"> -->
          <v-hover v-slot:default="{ hover }">
            <v-img
              :aspect-ratio="1"
              :src="`http://image.tmdb.org/t/p/w342/${associated.poster_path}`"
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
                    <div class="body-2 mb-2 font-weight-bold title my-text">{{ associated.title }}</div>

                    <div class="mt-1 mb-4 d-flex justify-space-between">
                      <span class="mr-2 my-text">{{ associated.release_date }}</span>
                      <v-avatar
                        class="white--text caption my-text"
                        small
                        size="30"
                        color="indigo"
                      >{{ associated.vote_average }}</v-avatar>
                    </div>

                    <div class="caption text-overview my-text">{{ associated.overview }}</div>
                    <v-btn :to="`/movies/${associated.id}`" small color="success">
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
    </v-container>
  </section>
</template>

<script>
export default {
  name: "index",
  data() {
    return {
      castDetails: null,
      associatedMovie: null
    };
  },

  async created() {
    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/person/${this.$route.params.id}?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US`
      );
      let res = await req.data;
      this.castDetails = res;
      console.log(res);
    } catch (error) {
      console.log(error);
    }

    try {
      let req = await this.$axios.get(
        `https://api.themoviedb.org/3/person/${this.$route.params.id}/movie_credits?api_key=62722bcd117e9d14d0bed44381ccf36f&language=en-US `
      );
      let res = await req.data;
      this.associatedMovie = res.cast;
    } catch (error) {
      console.log(error);
    }
  },
  computed: {
    checkGender() {
      return this.castDetails.gender == 1 ? "Female" : "Male";
    },
    getCast(castID) {
      this.$router.push(`/cast/${castID}`);
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
.rounded-profile {
  cursor: pointer;
  border-radius: 20px 20px 8px 8px;
}
.profile-image {
  border-radius: 8px 8px 0 0;
}
.mytext {
  font-family: "Sriracha", cursive !important;
}

.rounded-poster {
  border-radius: 1.5rem;
  max-width: 350px;
}

@media screen and (min-width: 1264px) {
  .go-down {
    margin-top: 4rem;
  }
}
</style>
