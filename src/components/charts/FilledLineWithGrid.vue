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
    </g>
  </svg>
</template>

<script>
import chartData from "@/components/charts/ChartMock.json";
import * as d3 from "d3-scale";
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
    // длина оси Y = высота контейнера svg - отступ сверху и снизу
    yAxisLength() {
      return this.height - 2 * this.margin;
    },
    // длина оси X= ширина контейнера svg - отступ слева и справа
    xAxisLength() {
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
      this.scaleX = d3
        .scaleLinear()
        .domain([0, 100])
        .range([0, this.xAxisLength]);

      this.scaleY = d3
        .scaleLinear()
        .domain([100, 0])
        .range([0, this.yAxisLength]);
      console.log(this.scaleX);
      console.log(this.scaleY);
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

<style scoped>
.tick {
  opacity: 1;
}
.path,
.line {
  fill: none;
  stroke: #333;
  stroke-width: 1px;
  stroke-dasharray: 2;
}
.grid-line {
  stroke: #000;
  stroke-opacity: 0.2;
}
text {
  font: 14px Verdana;
  fill: #7892b6;
}
.area {
  fill: lightblue;
  opacity: 50%;
}
.area-border {
  stroke: steelblue;
  stroke-width: 2;
}
.container {
  max-width: 510px;
  max-height: 500px;
}
</style>
