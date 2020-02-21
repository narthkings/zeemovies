<template>
  <div>
    <v-form @submit.prevent="SubmitedQuery">
      <v-text-field
        label="search for a movie"
        class="white--text mx-auto"
        append-icon="mdi-magnify"
        hide-details
        outlined
        dense
        dark
        v-model="searchedText"
      ></v-text-field>
    </v-form>
  </div>
</template>
<script>
export default {
  name: "mobileSearch",
  data() {
    return {
      searchedText: ""
    };
  },
  methods: {
    SubmitedQuery() {
      this.$router.push(`/search/${this.searchedText}`);
      this.searchedText = "";
    }
  }
};
</script>
<style >
/* .; */
</style>
