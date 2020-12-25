<template>
  <svg class="container" :viewBox="viewBox">
    <g class="x-axis" :transform="transformXParams">
      <x-axis />
    </g>
    <g class="y-axis" :transform="transformYParams">
      <y-axis />
    </g>
  </svg>
</template>

<script>
import chartData from "@/components/charts/ChartMock.json";
import * as d3 from "d3-scale";
export default {
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
  },
  methods: {
    addAxisY(chartData) {
      // функция интерполяции значений на ось Y
      const scaleY = d3.scale
        .linear()
        .domain([100, 0])
        .range([0, this.yAxisLength]);
      // создаем ось Y
      const yAxis = d3.svg
        .axis()
        .scale(scaleY)
        .orient("left");
    },
    addAxisX(chartData) {
      // функция интерполяции значений на ось Х
      const scaleX = d3.scale
        .linear()
        .domain([0, 100])
        .range([0, this.xAxisLength]);

      // функция интерполяции значений на ось Y
      const scaleY = d3.scale
        .linear()
        .domain([100, 0])
        .range([0, this.yAxisLength]);
      for (let i = 0; i < chartData.length; i++)
        chartData.push({
          x: scaleX(chartData[i].x) + this.margin,
          y: scaleY(chartData[i].y) + this.margin,
        });
    },
    drawChart() {
      // создаем ось X
      const xAxis = d3.svg
        .axis()
        .scale(this.scaleX)
        .orient("bottom");

      // отрисовка оси Y
      container
        .append("g")
        .attr("class", "y-axis")
        .attr("transform", "translate(" + this.margin + "," + this.margin + ")")
        .call(this.yAxis);

      // создаем набор вертикальных линий для сетки
      d3.selectAll("g.x-axis g.tick")
        .append("line")
        .classed("grid-line", true)
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", 0)
        .attr("y2", -(this.height - 2 * this.margin));

      // рисуем горизонтальные линии
      d3.selectAll("g.y-axis g.tick")
        .append("line")
        .classed("grid-line", true)
        .attr("x1", 0)
        .attr("y1", 0)
        .attr("x2", this.thxAxisLength)
        .attr("y2", 0);

      // функция, создающая по массиву точек линии
      const line = d3.svg
        .line()
        .x(function(d) {
          return d.x;
        })
        .y(function(d) {
          return d.y;
        });
      // функция, создающая область
      const area = d3.svg
        .area()
        .x(function(d) {
          return d.x;
        })
        .y0(this.height - this.margin)
        .y1(function(d) {
          return d.y;
        });

      const g = container.append("g");
      g.append("path")
        .attr("d", area(chartData))
        .style("fill", "lightblue");
      g.append("path")
        .attr("d", line(chartData))
        .style("stroke", "steelblue")
        .style("stroke-width", 2);
    },
  },
};
</script>

<style scoped>
.axis path,
.axis line {
  fill: none;
  stroke: #333;
}
.axis .grid-line {
  stroke: #000;
  stroke-opacity: 0.2;
}
.axis text {
  font: 10px Verdana;
}
.container {
  height: 500px;
  width: 500px;
  margin: 30px;
}
</style>
