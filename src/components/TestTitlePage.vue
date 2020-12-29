<template>
  <div>
    <div class="hello">
      <img alt="Vue logo" src="@/assets/logo.png" />
      <h1>{{ msg }}</h1>
    </div>
    <b-button class="button-to-chart" @click.prevent="setGraphVisible">
      <div class="button-text">{{ "Show chart with gradient area" }}</div>
    </b-button>
    <filled-line-with-grid v-if="showGraph" />
  </div>
</template>

<script>
import { BButton } from "bootstrap-vue";
import FilledLineWithGrid from "@/components/charts/FilledLineWithGrid";
export default {
  name: "TestTitlePage",
  components: {
    BButton,
    FilledLineWithGrid,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      showGraph: false,
    };
  },
  methods: {
    setGraphVisible() {
      this.showGraph = !this.showGraph;
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
