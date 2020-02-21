<template>
  <div>
    <v-form @submit.prevent="SubmitedQuery">
      <v-text-field
        :append-icon-cb="() => {}"
        v-model="searchedText"
        class="mr-auto white--text"
        placeholder="Search for movies..."
        single-line
        append-icon="mdi-magnify"
        color="white"
        hide-details
      />
    </v-form>
  </div>
</template>
<script>
export default {
  name: "searchInput",
  data() {
    return {
      searchedText: ""
    };
  }
};
</script>
<style>
</style>