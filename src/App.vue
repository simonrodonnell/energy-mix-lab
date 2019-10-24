<template lang="html">
  <div id="app-wrapper">
    <h3>Energy Mix Lab</h3>
    <div id="input-dates">
      <input type="date" v-model="inputStartDate">
      <!-- <input type="date" v-model="inputEndDate"> -->
      <input @click="handleClick" type="submit" value="Change Date">
    </div>
    <div id="energy-chart-wrapper">
      <GChart
      :type="this.chartStyle"
      :data="this.energiesChartData"
      :options="chartOptions"
      />
    </div>
    <div id="chart-style-select">
      <select v-model="chartStyle">
        <option value="PieChart">Pie chart</option>
        <option value="LineChart">Line chart</option>
        <option value="ColumnChart">Column chart</option>
        <option value="BarChart">Bar chart</option>
        <option value="AreaChart">Area chart</option>
        <option value="ScatterChart">Scatter chart</option>
      </select>

    </div>
  </div>
</template>


<script>
import {eventBus} from './main.js'
import EnergyChart from './components/EnergyChart.vue'
import { GChart } from "vue-google-charts";

export default {
  data(){
    return {
      energiesObject: [],
      url: 'https://api.carbonintensity.org.uk/generation',
      inputStartDate: "",
      inputEndDate: "",
      chartStyle: "PieChart",
      chartOptions: {
        chart: {
          title: "All UK Energy",
          subtitle: ""
        }
      }
    }
  },
  methods: {
    handleClick(){
      fetch(this.apiUrl)
      .then(res => res.json())
      .then(energies => this.energiesObject = energies.data[1].generationmix)
    }
  },
  components: {
    GChart,
    "energy-chart": EnergyChart
  },
  computed: {
    energiesArray: function(){
      return this.energiesObject.map((energy) => {
        return [energy.fuel, energy.perc]
      })
    },
    energiesChartData: function(){
      const chartHeader = [["Fuel", "Percentage"]];
      return chartHeader.concat(this.energiesArray);
    },
    apiUrl: function(){
      if (this.inputStartDate) {
        let startDate = this.inputStartDate.toString()
        // let endDate = this.inputEndDate.toString();
        return this.url + `/${startDate}T12:00Z` + `/${startDate}T12:30Z`
      } else {
        return this.url;
      };
    }
  },
  mounted(){
    fetch(this.apiUrl)
    .then(res => res.json())
    .then(energies => this.energiesObject = energies.data.generationmix)
    // console.log("object to array test:", energiesArray)

    // eventBus.$on('energy-selected', (energy) => {
    //   this.selectedEnergy = energy
    // })
  }
}
</script>


<style lang="css">
  input {
    border-radius: 100px;
  }
  h3 {
    color: salmon;
    font-size: 50px;
    text-decoration: underline;
    text-shadow: 5px 5px 5px brown;
    font-style: italic;
  }
  body {
    background-color: dodgerblue;
    background-image: url(https://3.bp.blogspot.com/_UA_EyvZSFHI/TOR5MDBXvWI/AAAAAAAAAjA/F4x5snJMy64/s1600/Dino0001.JPG);
  }
#energy-chart-wrapper {
    width: 50%;
    /* margin: 0 auto; */
  }
</style>
