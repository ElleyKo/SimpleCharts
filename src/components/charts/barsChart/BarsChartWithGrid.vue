<template>
  <div class="chart-container">
    <!--TODO display mobile change style-->
    <chart-title :chartTitle="chartTitle" />
    <svg class="container" v-if="isLoaded" :viewBox="viewBox">
      <axes-titles xTitle="Years" yTitle="Stock Price" />
      <chart-grid
        :height="height"
        :width="xAxisLength"
        :margin="margin"
        :chartData="$options.chartData"
        :scaleX="scaleX"
        :scaleY="scaleY"
        :addStrictXLine="true"
      />

      <g :transform="transformYParams">
        <rect
          class="bar"
          v-for="(point, index) in $options.chartData"
          :key="index"
          :x="scaleX(point.x) - barWidth() / 2"
          :y="scaleY(point.y)"
          :height="barHeight(point.y)"
          :width="barWidth()"
        ></rect>
        <text
          v-for="(point, index) in $options.chartData"
          :key="'point' + index"
          class="tooltip"
          :dy="scaleY(point.y)"
          y="-5"
          :x="scaleX(point.x)"
          style="text-anchor: middle;"
        >
          {{ point.y }}
        </text>
      </g>
      <defs>
        <linearGradient id="area-gradient" gradientTransform="rotate(90)">
          <stop offset="0%" :stop-color="getRandomColorOne" />
          <stop offset="50%" :stop-color="getRandomColorTwo" />
          <stop offset="100%" :stop-color="getRandomColorThree" />
        </linearGradient>
      </defs>
    </svg>
  </div>
</template>

<script>
import "@/assets/css/bar-chart.css";
import "@/assets/css/common.css";
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
      default: 500,
    },
    margin: {
      type: Number,
      default: 30,
    },
    chartTitle: {
      type: String,
      default: "XYZ Foods Stock Price",
    },
  },
  computed: {
    getRandomColorOne() {
      return "#" + this.randomPart() + this.randomPart() + this.randomPart();
    },
    getRandomColorTwo() {
      return "#" + this.randomPart() + this.randomPart() + this.randomPart();
    },
    getRandomColorThree() {
      return "#" + this.randomPart() + this.randomPart() + this.randomPart();
    },
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
      return this.height - 2 * this.margin;
    },
    xAxisLength() {
      return this.width - 2 * this.margin;
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
    randomPart() {
      const hex = Math.floor(Math.random() * 256).toString(16);
      return ("0" + String(hex)).substr(-2);
    },
    barWidth() {
      return this.scaleX.bandwidth();
    },
    barHeight(y) {
      return this.yAxisLength - this.scaleY(y);
    },
    setScales() {
      const chartData = this.$options.chartData;
      this.scaleX = d3Scale
        .scaleBand()
        .range([this.zeroValue, this.width])
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
