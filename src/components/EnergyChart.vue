<template lang="html">
  <div v-if="energiesArray" id="energyChart">
    <div id="chartWrapper">
      <p>{{energiesArray}}</p>
      <GChart
      type="ColumnChart"
      :data="chartData"
      :options="chartOptions"
      />
    </div>
  </div>
</template>


<script>
import { eventBus } from '../main.js'
import { GChart } from "vue-google-charts";
export default {
  name: "energy-chart",
  components: {
    GChart
  },
  props: ["energiesArray"],
  data(){
    return {
      chartData: [
        ["Fuel", "Percentage"],
        ["Biomass", 5.2],
        ["Coal", 1],
        ["imports", 6.3]
      ],
      chartOptions: {
        chart: {
          title: "UK Energy Mix",
          subtitle: ""
        }
      }
    };
  }
}
</script>


<style lang="css" scoped>
</style>
