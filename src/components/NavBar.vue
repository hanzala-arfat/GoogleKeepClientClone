<template>
  <div class="NavBar-Page">
    <div>
      <nav class="navbar navbar-dark bg-info justify-content-between">
        <a class="navbar-brand">Keep Data</a>
        <form class="form-inline">
          <input
            class="form-control mr-sm-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            v-model="searchingData"
          >
          <button
            v-if="!searching"
            class="btn btn-outline-light my-2 my-sm-0"
            type="submit"
            @click="search"
          >Search</button>
          <button
            v-else
            class="btn btn-outline-light my-2 my-sm-0"
            type="submit"
            @click="Cancel"
          >Cancel</button>
        </form>
      </nav>
    </div>
  </div>
</template>

<script>
export default {
  name: "NavBar",
  props: ["sendtoNavebar"],
  data() {
    return {
      searchingData: "",
      searching: false
    };
  },
  methods: {
    search() {
      this.$emit(
        "SearchData",
        {
          searchData: this.searchingData,
          searching: true
        },
        (this.searching = true)
      ); //app.vue ko bheja gya h
      this.searchingData = "";
    },
    Cancel() {
      this.$emit("SearchData", { searching: false }, 
      this.searching = false);
    }
  }
};
</script>

<style scoped>
.navbar-brand {
  color: white !important;
  font-weight: bolder;
}
</style>
