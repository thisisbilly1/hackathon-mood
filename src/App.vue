<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import {
  Chart as ChartJS,
  RadialLinearScale,
  ArcElement,
  Tooltip,
} from 'chart.js'
import { PolarArea } from 'vue-chartjs'

ChartJS.register(RadialLinearScale, ArcElement, Tooltip)

function getBackgroundColor(color, alpha = 0.5) {
  const opacity = Math.floor(alpha * 255);
  return color + opacity.toString(16).toUpperCase();
}

const moods = [
  {
    label: 'healthy',
    backgroundColor: getBackgroundColor('#07B151'),
  },
  {
    label: 'coping',
    backgroundColor: getBackgroundColor('#FCED23'),
  },
  {
    label: 'struggling',
    backgroundColor: getBackgroundColor('#F6851E'),
  },
  {
    label: 'unwell',
    backgroundColor: getBackgroundColor('#D52127'),
  },
  {
    label: 'depressed',
    backgroundColor: getBackgroundColor('#733B97'),
  },
]

const data = {
  labels: moods.map(m => m.label),
  datasets: [
    {
      label: 'test',
      data: [1,2,3,4,5],
      backgroundColor: moods.map(m => m.backgroundColor),
    },
  ],
};
const options = {
  responsive: true,
  maintainAspectRatio: false,
  // elements: {
  //   arc: {
  //     angle: 180 / moods.length
  //   }
  // },
  scales: {
    r: {
      // startAngle: -90,
      grid: {
        color: 'white',
      },
      ticks: {
        display: false,
        stepSize: 1,
      }
    }
  }
};
</script>

<template>
  <PolarArea
    style="height: 400px"
    :data="data"
    :options="options"
  />
</template>

<style scoped>
</style>
