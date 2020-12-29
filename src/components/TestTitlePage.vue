<template>
  <div>
    <div class="hello">
      <img alt="Vue logo" src="@/assets/logo.png" />
      <h1>{{ msg }}</h1>
    </div>
    <div class="chart-with-btn">
      <b-button class="button-to-chart" @click.prevent="setAreaChartVisible">
        <div class="button-text">{{ "Show chart with gradient area" }}</div>
      </b-button>
      <area-chart-with-grid v-if="showArea" />
    </div>
    <div class="chart-with-btn">
      <b-button class="button-to-chart" @click.prevent="setBarsChartVisible">
        <div class="button-text">{{ "Show chart with bars" }}</div>
      </b-button>
      <bars-chart-with-grid v-if="showBars" />
    </div>
  </div>
</template>

<script>
import { BButton } from "bootstrap-vue";
import AreaChartWithGrid from "@/components/charts/areaChart/AreaChartWithGrid";
import BarsChartWithGrid from "@/components/charts/barsChart/BarsChartWithGrid";
export default {
  name: "TestTitlePage",
  components: {
    BButton,
    AreaChartWithGrid,
    BarsChartWithGrid,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      showArea: false,
      showBars: false,
    };
  },
  methods: {
    setAreaChartVisible() {
      this.showArea = !this.showArea;
    },
    setBarsChartVisible() {
      this.showBars = !this.showBars;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
