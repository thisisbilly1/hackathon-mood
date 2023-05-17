<template>
  <div class="line-chart mt-5">
    <Line :data="chartData" :options="options" />
  </div>
</template>
<script>
import { computed, toRefs } from "vue";
import fakeData from "../fakeData";
import { useTheme } from "vuetify";

import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from "chart.js";
import { Line } from "vue-chartjs";

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

export default {
  components: { Line },
  props: {
    moods: {
      type: Array,
      required: true,
    },
  },
  setup(props) {
    const { moods } = toRefs(props);
    const theme = useTheme();
    const dark = computed(() => {
      return theme.global.name.value === "dark";
    });

    const chartData = computed(() => ({
      labels: fakeData.map((t) => t.time),
      datasets: moods.value.map((mood, moodIndex) => {
        return {
          label: mood.label,
          backgroundColor: mood.color,
          borderColor: mood.backgroundColor,
          color: mood.backgroundColor,
          data: fakeData.reduce((prev, timedData) => {
            const mapped = timedData.data.map((d) =>
              d.mood === moodIndex ? 1 : 0
            );
            const sum = mapped.reduce((a, b) => a + b, 0);
            return [...prev, sum];
          }, []),
        };
      }),
    }));

    const options = computed(() => ({
      responsive: true,
      maintainAspectRatio: false,
      scales: {
        x: {
          grid: {
            color: dark.value ? "white" : "black",
          },
          ticks: {
            color: dark.value ? "white" : "black",
          }
        },
        y: {
          grid: {
            color: dark.value ? "white" : "black",
          },
          ticks: {
            color: dark.value ? "white" : "black",
          }
        },
      },
      plugins: {
        tooltip: {
          intersect: false,
        },
        legend: {
          labels: {
            color: dark.value ? "white" : "black",
          },
        },
      },
    }));

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
