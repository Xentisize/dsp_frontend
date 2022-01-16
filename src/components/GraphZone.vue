<template>
  <div>
    <label for="x">X-axis</label>
    <select class="form-select" id="x" v-model="x">
      <option v-for="(o, i) in meta['columns']" :key="i">{{ o }}</option>"
    </select>

    <p>{{ x_data }}</p>

    <label for="y">Y-axis</label>
    <select class="form-select" id="y" v-model="y">
      <option v-for="(o, i) in meta['columns']" :key="i">{{ o }}</option>
    </select>

    <p>{{ y_data }}</p>

    <label for="graph-type">Graph Type</label>
    <select class="form-select" id="graph-type" v-model="graphType">
      <option value="bar">Bar Chart</option>
      <option value="line">Line Chart</option>
      <option value="pie">Pie Chart</option>
      <option value="scatter">Scatter Diagram</option>
    </select>

    <canvas id="myChart"></canvas>
  </div>
</template>

<script>
import Chart from "chart.js/auto"

export default {
  name: "GraphZone",

  data() {
    return {
      x: "",
      x_data: [],
      y: "",
      y_data: [],
      graphType: "bar",
      chart: ""
    }
  },

  props: {
    meta: Object,
  },

  methods: {
    drawChart() {
      const ctx = document.getElementById("myChart")
      if (this.chart) {
        this.chart.destroy()
      }

      this.chart = new Chart(ctx, {
        type: this.graphType || "bar",

        data: {
          labels: this.x_data,

          datasets: [{
            label: `${this.y} against ${this.x}`,
            data: this.y_data,
            backgroundColor: ['#a6cee3', '#1f78b4', '#b2df8a', '#33a02c', '#fb9a99', '#e31a1c', '#fdbf6f', '#ff7f00', '#cab2d6', '#6a3d9a', '#ffff99', '#b15928'],
          }],
        },

        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          },

          plugins: {
            colorschemes: {
              scheme: "brewer.Paired12"
            }
          }
        }
      })
    },

    fetch_x_data() {
      let that = this
      fetch("http://localhost:8000/data/get_data?file=" + this.meta["name"] + "&label=" + this.x).then(
        response => response.json()
      ).then(data => that.x_data = data)
    },

  },

  watch: {
    graphType: function() {
      this.drawChart()
    },

    x: function() {
      fetch("http://localhost:8000/data/get_data?file=" + this.meta["name"] + "&label=" + this.x).then(
        response => response.json()).then(
          data => {
            this.x_data = data
            if (this.x_data !== [] && this.y_data !== []) {
              this.drawChart()
            }
          }
        )
    },

    y: function() {
      fetch("http://localhost:8000/data/get_data?file=" + this.meta["name"] + "&label=" + this.y).then(
        response => response.json()).then(
          data => {
            this.y_data = data
            if (this.x_data !== [] && this.y_data !== []) {
              this.drawChart()
            }
          }
        )
    }
  }
}
</script>

<style>

</style>