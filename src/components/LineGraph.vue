<template>
  <div class="line-chart mt-5">
    <Line :data="chartData" :options="options" />
  </div>
</template>
<script>
import { computed, toRefs } from 'vue';
import fakeData from '../fakeData';

import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)

export default {
  components: { Line },
  props: {
    moods: {
      type: Array,
      required: true,
    }
  },
  setup(props) {
    const { moods } = toRefs(props);

    const chartData = computed(() => ({
      labels: fakeData.map(t => t.time),
      datasets: moods.value.map((mood, moodIndex) => {
        return {
          label: mood.label,
          backgroundColor: mood.color,
          borderColor: mood.backgroundColor,
          color: mood.backgroundColor,
          data: fakeData.reduce((prev, timedData) => {
            const mapped = timedData.data.map(d => d.mood === moodIndex ? 1 : 0);
            const sum = mapped.reduce((a, b) => a + b, 0);
            return [...prev, sum];
          }, []),
        }
      }),
    }));

    const options = {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
          tooltip: {
            intersect: false,
          },
        },
    };

    
    return {
      chartData,
      options,
    };
  },
};
</script>

<style scoped>
.line-chart {
  max-width: 100vw;
  width: 850px;
  height: 400px;
  margin-left: auto;
  margin-right: auto;
}
</style>
