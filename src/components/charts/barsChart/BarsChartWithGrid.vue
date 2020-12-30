<template>
  <svg class="container" v-if="isLoaded" :viewBox="viewBox">
    <axes-titles xTitle="Years" yTitle="Stock Price" />
    <chart-grid
      :height="height"
      :width="width"
      :margin="margin"
      :chartData="$options.chartData"
      :scaleX="scaleX"
      :scaleY="scaleY"
    />
    <chart-title :chartTitle="chartTitle" :margin="margin" />
    <g transform="translate(100,100)">
      <rect
        class="bar"
        v-for="(point, index) in $options.chartData"
        :key="index"
        :x="scaleX(point.x)"
        :y="scaleY(point.y)"
        :height="barHeight(point.y)"
        :width="barWidth()"
      ></rect>
    </g>
    <defs>
      <linearGradient id="area-gradient" gradientTransform="rotate(90)">
        <stop offset="0%" stop-color="#f5ff1e" />
        <stop offset="50%" stop-color="#fbb91c" />
        <stop offset="100%" stop-color="#174b93" />
      </linearGradient>
    </defs>
  </svg>
</template>

<script>
import chartData from "@/components/charts/barsChart/BarsChartMock.json";
import ChartTitle from "@/components/common/ChartTitle";
import ChartGrid from "@/components/common/ChartGrid";
import AxesTitles from "@/components/common/AxesTitles";
import * as d3 from "d3";
import * as d3Scale from "d3-scale";
// import * as d3Shape from "d3-shape";
export default {
  chartData,
  name: "BarsChartWithGrid",
  components: { ChartGrid, ChartTitle, AxesTitles },
  props: {
    height: {
      type: Number,
      default: 500,
    },
    width: {
      type: Number,
      default: 600,
    },
    margin: {
      type: Number,
      default: 200,
    },
    chartTitle: {
      type: String,
      default: "XYZ Foods Stock Price",
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
    viewBox() {
      return `0 0 ${this.width} ${this.height}`;
    },
    yAxisLength() {
      return this.height - 250;
    },
    xAxisLength() {
      return this.width - this.margin;
    },
    y2ValueXAxis() {
      return -(this.height - 2 * this.margin);
    },
  },
  data() {
    return {
      isLoaded: false,
      zeroValue: 0,
      maxDomain: 100,
      scaleX: [],
      scaleY: [],
    };
  },
  methods: {
    barWidth() {
      return this.scaleX.bandwidth();
    },
    barHeight(point) {
      const h = this.height - this.scaleY(point.y);
      console.log(this.height);
      console.log(this.scaleY(point.y));
      return h;
    },
    setScales() {
      const chartData = this.$options.chartData;
      this.scaleX = d3Scale
        .scaleBand()
        .range([this.zeroValue, this.xAxisLength])
        .padding(0.4)
        .domain(chartData.map((d) => d.x));

      this.scaleY = d3Scale
        .scaleLinear()
        .range([this.yAxisLength, 0])
        .domain([0, d3.max(chartData, (d) => d.y)]);
    },
    getX(d) {
      return d.x;
    },
    getY(d) {
      return d.y;
    },
    getTooltipText(x, y) {
      return `x: ${x}, y: ${y}`;
    },
  },
  mounted() {
    this.setScales();
    this.isLoaded = true;
  },
};
</script>

<style src="@/assets/css/bar-chart.css"></style>
