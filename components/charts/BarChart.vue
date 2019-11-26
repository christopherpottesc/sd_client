<template>
  <BarChart :chartdata="chartdata"/>
</template>

<script>
import BarChart from "./BarChart.js";

export default {
  components: {
    BarChart
  },
  props: {
    chartdata: {
      type: Object,
      default: () => {}
    }
  }
}
</script>
