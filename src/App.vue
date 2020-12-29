<template>
  <div class="main-app-style">
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

<style src="@/assets/css/app.css"></style>
