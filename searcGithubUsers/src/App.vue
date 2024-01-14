<template>
  <div class="container">
    <Search />
    <Lists />
  </div>
</template>

<script>
import Search from "./components/Search.vue";
import Lists from "./components/Lists.vue";
export default {
  name: "App",
  components: {
    Search,
    Lists,
  },
};
</script>