<template>
  <fragment>
    <g class="x-axis" :transform="transformXParams">
      <g
        class="tick"
        v-for="point in chartData"
        :transform="'translate(' + scaleX(point.x) + ', 0)'"
        :key="point.x"
      >
        <line class="axis-mark" y2="4"></line>
        <line y2="6" x2="0"></line>
        <text dy=".71em" y="9" x="0" style="text-anchor: middle;">
          {{ point.x }}
        </text>
        <line
          class="grid-line"
          v-if="showGrid"
          x1="0"
          y1="0"
          x2="0"
          y2="-440"
        ></line>
      </g>
    </g>
    <g class="y-axis" :transform="transformYParams">
      <g
        class="tick"
        v-for="point in chartData"
        :transform="'translate(0, ' + scaleY(point.x) + ')'"
        style="opacity: 1;"
        :key="point.x"
      >
        <line class="axis-mark" x2="-4"></line>
        <line x2="-6" y2="0"></line>
        <text dy=".32em" x="-5" y="0" style="text-anchor: end;">
          {{ point.x }}
        </text>
        <line
          class="grid-line"
          v-if="showGrid"
          x1="0"
          y1="0"
          x2="440"
          y2="0"
        ></line>
      </g>
    </g>
  </fragment>
</template>

<script>
import { Fragment } from "vue-fragment";
export default {
  name: "FilledLineWithGrid",
  components: { Fragment },
  props: {
    chartData: {
      type: Array,
      default: () => {},
    },
    height: {
      type: Number,
      default: 500,
    },
    width: {
      type: Number,
      default: 500,
    },
    margin: {
      type: Number,
      default: 30,
    },
    scaleX: {
      type: Function,
    },
    scaleY: {
      type: Function,
    },
    showGrid: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    transformXParams() {
      return (
        "translate(" + this.margin + "," + (this.height - this.margin) + ")"
      );
    },
    transformYParams() {
      return "translate(" + this.margin + "," + this.margin + ")";
    },
  },
};
</script>

<style src="@/assets/css/area-chart.css"></style>
