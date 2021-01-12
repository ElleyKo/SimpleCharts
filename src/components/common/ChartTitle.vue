<template>
  <span class="chart-title">{{ chartTitle }}</span>
</template>

<script>
export default {
  name: "ChartTitle",
  components: {},
  props: {
    chartTitle: {
      type: String,
      default: "",
    },
  },
};
</script>

<style src="@/assets/css/common.css"></style>
