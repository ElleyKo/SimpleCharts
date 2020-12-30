<template>
  <g>
    <text class="axis-title" y="50" x="300" v-if="hasXTitle">
      {{ xTitle }}
    </text>
    <text
      class="axis-title"
      transform="rotate(-90)"
      y="6"
      dy="-5.1em"
      v-if="hasYTitle"
    >
      {{ yTitle }}
    </text>
  </g>
</template>

<script>
export default {
  name: "AxesTitles",
  components: {},
  props: {
    yTitle: {
      type: String,
      default: "",
    },
    xTitle: {
      type: String,
      default: "",
    },
  },
  computed: {
    hasXTitle() {
      return !!this.xTitle;
    },
    hasYTitle() {
      return !!this.yTitle;
    },
  },
};
</script>

<style src=""></style>
