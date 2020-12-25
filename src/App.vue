<template>
  <div id="app">
    <test-title-page msg="Welcome to the simple charts app" />
  </div>
</template>

<script>
import TestTitlePage from "@/components/TestTitlePage.vue";
import "@/assets/css/app.css";
export default {
  name: "App",
  components: {
    TestTitlePage,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
