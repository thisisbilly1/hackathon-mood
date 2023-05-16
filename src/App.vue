<template>
  <v-app>
    <div class="chart mt-5">
      <PolarArea :data="chartData" :options="options" />
    </div>
    <div class="slider mt-5">
      <v-slider
        v-model="time"
        :min="0"
        :max="5"
        :step="1"
        thumb-label
      />
    </div>
  </v-app>
</template>
<script>
import { ref, computed } from 'vue';
import fakeData from './fakeData';

import {
  Chart as ChartJS,
  RadialLinearScale,
  ArcElement,
  Tooltip,
} from "chart.js";
import { PolarArea } from "vue-chartjs";
ChartJS.register(RadialLinearScale, ArcElement, Tooltip);

export default {
  components: { PolarArea },
  setup() {
    function getBackgroundColor(color, alpha = 0.5) {
      const opacity = Math.floor(alpha * 255);
      return color + opacity.toString(16).toUpperCase();
    }

    const moods = [
      {
        label: "healthy",
        backgroundColor: getBackgroundColor("#07B151"),
      },
      {
        label: "coping",
        backgroundColor: getBackgroundColor("#FCED23"),
      },
      {
        label: "struggling",
        backgroundColor: getBackgroundColor("#F6851E"),
      },
      {
        label: "unwell",
        backgroundColor: getBackgroundColor("#D52127"),
      },
      {
        label: "depressed",
        backgroundColor: getBackgroundColor("#733B97"),
      },
    ];
    
    const time = ref(0);

    const data = computed(() => {
      const timedData = fakeData.find(x => x.time === time.value);
      if (!timedData) return [];
      // aggregate the moods
      const data = new Array(moods.length).fill(0);
      timedData.data.forEach(person => {
        data[person.mood] += 1;
      });
      return data;
    });

    const chartData = computed(() => ({
      labels: moods.map((m) => m.label),
      datasets: [
        {
          label: "test",
          data: data.value,
          backgroundColor: moods.map((m) => m.backgroundColor),
        },
      ],
    }));

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
            color: "white",
          },
          ticks: {
            display: false,
            stepSize: 1,
          },
        },
      },
    };

    
    return {
      chartData,
      options,
      time,
    };
  },
};
</script>

<style scoped>
.chart {
  width: 400px;
  margin-left: auto;
  margin-right: auto;
}
.slider {
  width: 400px;
  margin: auto;
}
</style>
