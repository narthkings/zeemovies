<template>
  <v-app dark>
    <v-navigation-drawer
      class="blue darken-3"
      v-model="drawer"
      fixed
      app
      v-if="this.$vuetify.breakpoint.xsOnly "
    >
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact>
          <v-list-item-action>
            <v-icon color="white">{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="white--text" v-text="item.title" />
          </v-list-item-content>
        </v-list-item>

        <v-list-item>
          <v-list-item-content>
            <mobileSearch />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar class="blue darken-3" fixed app>
      <v-toolbar-title class="subtitle-1 font-weight-bold" v-if="this.$vuetify.breakpoint.smAndUp ">
        <nuxt-link to="/rated" class="link white--text">Top Rated</nuxt-link>
        <nuxt-link to="/upcoming" class="link white--text ml-4">Upcoming</nuxt-link>
      </v-toolbar-title>

      <v-spacer v-if="this.$vuetify.breakpoint.smAndUp " />

      <v-row v-if="this.$vuetify.breakpoint.xsOnly ">
        <v-app-bar-nav-icon color="white" @click.stop="drawer = !drawer" />
      </v-row>

      <!-- this is the space between the two element on the mobile view -->
      <v-spacer v-if="this.$vuetify.breakpoint.xsOnly " />

      <div class="title">
        <nuxt-link to="/" class="link logo white--text">ZEEMOVIES</nuxt-link>
      </div>

      <v-spacer v-if="this.$vuetify.breakpoint.smAndUp " />

      <span v-if="this.$vuetify.breakpoint.smAndUp ">
        <mobileSearch light />
      </span>
    </v-app-bar>
    <v-content>
      <!-- <v-container> -->
      <nuxt />
      <!-- </v-container> -->
    </v-content>

    <v-footer :fixed="fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import mobileSearch from "../components/mobileSearch";
export default {
  components: { mobileSearch },
  data() {
    return {
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-home",
          title: "Home",
          to: "/"
        },
        {
          icon: "mdi-youtube-subscription",
          title: "Upcoming Movies",
          to: "/Upcoming"
        },
        {
          icon: "mdi-library-movie",
          title: "Rated",
          to: "/rated"
        }
      ]
    };
  }
};
</script>
<style >
@import url("https://fonts.googleapis.com/css?family=Fondamento|Odibee+Sans|Russo+One|Sriracha&display=swap");
* {
  font-family: "Russo One", sans-serif;
}
.link {
  text-decoration: none;
}
</style>