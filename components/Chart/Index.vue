<template>
  <Line :chart-options="chartOptions" :chart-data="chartData" :chart-id="dataLabel" :dataset-id-key="dataLabel"
    :plugins="plugins" :css-classes="cssClasses" :styles="styles" :width="width" :height="height" />
</template>

<script>
import 'chart.js/auto';
import { Line } from 'vue-chartjs'

export default {
  name: 'BarChart',
  components: { Line },
  props: {
    chartId: {
      type: String,
      default: 'bar-chart'
    },
    inputData: {
      type: Array,
      required: true
    },
    datasetIdKey: {
      type: String,
      default: 'label'
    },
    globalSteps: {
      type: Number,
      default: 1000
    },
    dataLabel: {
      type: String,
      default: 'Default Label'
    },
    width: {
      type: Number,
      default: 100
    },
    height: {
      type: Number,
      default: 50
    },
    cssClasses: {
      default: '',
      type: String
    },
    stepSize: {
      default: 10,
      type: Number
    },
    styles: {
      type: Object,
      default: () => { }
    },
    plugins: {
      type: Object,
      default: () => { }
    }
  },
  computed: {
    chartOptions() {
      const maxCuts = this.inputData.map((item) => {
        return item.cuts;
      });

      const yMax = Math.max(...maxCuts) * 1.2;

      return {
        pointRadius: 0,
        responsive: true,
        scales: {
          y: {
            title: {
              display: true,
              text: 'cuts'
            },
            suggestedMin: 0,
            suggestedMax: yMax
          },
          x: {
            title: {
              display: true,
              text: 'steps'
            },
          }
        }
      }
    },
    calculateLabels() {
      const labels = []

      for (let i = 0; i < this.globalSteps + 1; i++) {
        labels.push(String(i))
      }

      return labels
    },
    calculateDatasets() {
      const dataPoints = []

      this.inputData.forEach(cut => {
        for (let i = 0; i < cut.steps; i++) {
          dataPoints.push(cut.cuts)
        }
      })

      return [{
        label: this.dataLabel,
        data: dataPoints,
        fill: true,
        backgroundColor: 'rgba(75, 192, 192, 0.4)',
        stepped: true
      }]
    },
    chartData() {
      return {
        labels: this.calculateLabels,
        datasets: this.calculateDatasets
      }
    }
  }
}
</script>