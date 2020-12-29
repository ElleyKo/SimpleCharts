<template>
  <svg class="container" v-if="isLoaded" :viewBox="viewBox">
    <g class="x-axis" :transform="transformXParams">
      <g
        class="tick"
        v-for="point in $options.chartData"
        :transform="'translate(' + scaleX(point.x) + ', 0)'"
        :key="point.x"
      >
        <line y2="6" x2="0"></line>
        <text dy=".71em" y="9" x="0" style="text-anchor: middle;">
          {{ point.x }}
        </text>
        <line class="grid-line" x1="0" y1="0" x2="0" y2="-440"></line>
      </g>
    </g>
    <g class="y-axis" :transform="transformYParams">
      <g
        class="tick"
        v-for="point in $options.chartData"
        :transform="'translate(0, ' + scaleY(point.x) + ')'"
        style="opacity: 1;"
        :key="point.x"
      >
        <line x2="-6" y2="0"></line>
        <text dy=".32em" x="-9" y="0" style="text-anchor: end;">
          {{ point.x }}
        </text>
        <line class="grid-line" x1="0" y1="0" x2="440" y2="0"></line>
      </g>
    </g>
    <g>
      <path :d="getArea" class="area"></path>
      <path :d="getLine" class="area-border"></path>
      <circle
        v-for="(point, index) in rawData"
        :key="index"
        :cx="point.x"
        :cy="point.y"
        :r="pointRadius"
        class="pointColor"
      ></circle>
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
import chartData from "@/components/charts/ChartMock.json";
import * as d3Scale from "d3-scale";
import * as d3Shape from "d3-shape";
export default {
  chartData,
  name: "FilledLineWithGrid",
  components: {},
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
    pointRadius: {
      type: Number,
      default: 3.5,
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
      // длина оси Y = высота контейнера svg - отступ сверху и снизу
      return this.height - 2 * this.margin;
    },
    xAxisLength() {
      // длина оси X= ширина контейнера svg - отступ слева и справа
      return this.width - 2 * this.margin;
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
  },
  mounted() {
    this.setScales();
    this.setRawData();
    this.isLoaded = true;
  },
};
</script>

<style src="@/assets/css/area-chart.css"></style>
