<template>
  <text class="bar-title" :x="margin" :y="50">{{ chartTitle }}</text>
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
    margin: {
      type: Number,
      default: 30,
    },
  },
};
</script>

<style src=""></style>
