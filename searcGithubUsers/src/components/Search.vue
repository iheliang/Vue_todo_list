<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="keyWord"
      />&nbsp;<button @click="searchUser">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyWord: "",
    };
  },
  methods: {
    searchUser() {
      this.$bus.$emit("updateListData", {
        isFirst: false,
        isLoading: true,
        errMsg: "",
        users: [],
      });
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        (response) => {
          console.log(response.data.items);
          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: "",
            users: response.data.items,
          });
        },
        (error) => {
          console.log(error.message);
          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: error.message,
            users: [],
          });
        }
      );
    },
  },
};
</script>

<style>
</style>