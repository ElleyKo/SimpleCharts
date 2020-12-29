<template>
  <svg class="container" v-if="isLoaded" :viewBox="viewBox">
    <chart-grid
      :height="height"
      :width="width"
      :margin="margin"
      :chartData="$options.chartData"
      :scaleX="scaleX"
      :scaleY="scaleY"
    />
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
      >
        <g class="tooltip-rect" transform="translate(350,200)">
          <rect
            class="tooltip-rect"
            x="-3em"
            y="-45"
            width="6em"
            height="1.25em"
          />
          <text
            class="tooltip-text"
            y="-45"
            dy="1em"
            text-anchor="middle"
            fill="ForestGreen"
          >
            {{ getTooltipText(point.x, point.y) }}
          </text>
        </g>
      </circle>
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
import chartData from "@/components/charts/areaChart/AreaChartMock.json";
import ChartGrid from "@/components/common/ChartGrid";
import * as d3Scale from "d3-scale";
import * as d3Shape from "d3-shape";
export default {
  chartData,
  name: "AreaChartWithGrid",
  components: { ChartGrid },
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

<style src="@/assets/css/area-chart.css"></style>
