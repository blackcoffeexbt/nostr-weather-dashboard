<template>
  <div>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: {
    chartData: {
      type: Object,
      required: true
    }
  },
  watch: {
    chartData: {
      handler() {
        this.renderChart(this.chartData, this.options)
      },
      deep: true
    }
  },
  mounted() {
    this.renderChart(this.chartData, this.options)
  },
  data() {
    return {
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          x: {
            type: 'time',
            time: {
              unit: 'minute'
            }
          },
          y: {
            beginAtZero: false
          }
        }
      }
    }
  }
}
</script>

<style scoped>
canvas {
  height: 25vh;
}
</style>
