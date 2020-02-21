<template>
  <div>
    <!-- my id ={{$route.params.id }} -->
    <singleMovie />
  </div>
</template>
<script>
import singleMovie from "~/components/singleMovie";
export default {
  name: "index",
  components: {
    singleMovie
  }
};
</script>
