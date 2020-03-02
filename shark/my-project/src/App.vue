<template>
  <div id="app">
    <div class="container">
      <section class="searchA">
        <h3 class="searchHead">Search Github Users</h3>
      </section>
    </div>
  </div>
</template>

<script>
import Search from "./components/search";
import List from "./components/list";
export default {
  components: {
    Search,
    List
  }
};
</script>

<style scoped></style>
