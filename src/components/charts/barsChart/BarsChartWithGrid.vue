<template>
  <svg class="container" v-if="isLoaded" :viewBox="viewBox">
    <chart-grid
      :height="height"
      :width="width"
      :margin="margin"
      :chartData="$options.chartData"
      :scaleX="scaleX"
      :scaleY="scaleY"
      xTitle="Years"
      yTitle="Stock Price"
    />
    <text class="bar-title" :x="margin" :y="50">{{ chartTitle }}</text>
    <g transform="translate(100,100)">
      <rect
        class="bar"
        v-for="(point, index) in rawData"
        :key="index"
        :x="scaleX(point.x)"
        :y="scaleY(point.y)"
        :height="barHeight(point.y)"
        :width="5"
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
import ChartGrid from "@/components/common/ChartGrid";
import * as d3Scale from "d3-scale";
import * as d3Shape from "d3-shape";
export default {
  chartData,
  name: "BarsChartWithGrid",
  components: { ChartGrid },
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
      return this.height - this.margin;
    },
    xAxisLength() {
      return this.width - this.margin;
    },
    y2ValueXAxis() {
      return -(this.height - 2 * this.margin);
    },
    getArea() {
      const area = d3Shape
        .area()
        .x((d) => d.x)
        .y0(this.height - this.margin)
        .y1((d) => d.y)
        .curve(d3Shape.curveCatmullRom.alpha(0.3));
      return area(this.rawData);
    },
    getLine() {
      const line = d3Shape
        .line()
        .x((d) => d.x)
        .y((d) => d.y)
        .curve(d3Shape.curveCatmullRom.alpha(0.3));
      return line(this.rawData);
    },
  },
  data() {
    return {
      isLoaded: false,
      zeroValue: 0,
      maxDomain: 100,
      scaleX: [],
      scaleY: [],
      rawData: [],
    };
  },
  methods: {
    setRawData() {
      const chartData = this.$options.chartData;
      const rawData = [];
      chartData.forEach((point) => {
        rawData.push({
          x: this.scaleX(point.x) + this.margin,
          y: this.scaleY(point.y) + this.margin,
        });
      });
      this.rawData = rawData;
    },
    barHeight(point) {
      return this.height - this.scaleY(point.y);
    },
    setScales() {
      this.scaleX = d3Scale
        .scaleLinear()
        .domain([this.zeroValue, this.maxDomain])
        .range([0, this.xAxisLength]);

      this.scaleY = d3Scale
        .scaleLinear()
        .domain([this.maxDomain, this.zeroValue])
        .range([0, this.yAxisLength]);
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
    this.setRawData();
    this.isLoaded = true;
  },
};
</script>

<style src="@/assets/css/bar-chart.css"></style>
