<template>
  <div class="chart-container">
    <div>
      <div class="chart mt-5">
        <PolarArea :data="chartData" :options="options" />
      </div>
      <div class="slider">
        <v-slider
          v-model="time"
          :min="0"
          :max="5"
          :step="1"
          thumb-label="always"
        >
          <template v-slot:thumb-label>
            <div class="slider-label">{{ label }}</div>
          </template>
        </v-slider>
      </div>
    </div>
    <v-card class="legend-container">
      <v-card-title>Moods</v-card-title>
      <v-card-text class="legend">
        <div v-for="mood in moods" :key="mood.label" class="legend-item">
          <v-sheet
            :color="mood.color"
            height="25"
            width="40"
          />
          {{mood.label}}
        </div>
      </v-card-text>
    </v-card>
  </div>
</template>
<script>
import { ref, computed, toRefs } from 'vue';
import fakeData from '../fakeData';

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
  props: {
    moods: {
      type: Array,
      required: true,
    }
  },
  setup(props) {
    const { moods } = toRefs(props);

    const time = ref(0);
    
    const label = computed(() => {
      return fakeData[time.value]?.time;
    });

    const data = computed(() => {
      const timedData = fakeData[time.value];
      if (!timedData) return [];
      // aggregate the moods
      const data = new Array(moods.value.length).fill(0);
      timedData.data.forEach(person => {
        data[person.mood] += 1;
      });
      return data;
    });

    const chartData = computed(() => ({
      labels: moods.value.map((m) => m.label),
      datasets: [
        {
          label: "test",
          data: data.value,
          backgroundColor: moods.value.map((m) => m.backgroundColor),
        },
      ],
    }));

    const options = {
      responsive: true,
      maintainAspectRatio: false,
      // elements: {
      //   arc: {
      //     angle: 180 / moods.value.length
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
      label,
      moods,
    };
  },
};
</script>

<style scoped>
.chart-container {
  display: flex;
  gap: 10px;
  margin: auto;
}
.chart {
  width: 400px;
  margin-left: auto;
  margin-right: auto;
}
.slider {
  width: 400px;
  margin: auto;
  margin-top: 50px;
}
.slider-label {
  white-space: nowrap;
}
.legend-container {
  height: fit-content;
  margin: auto;
}
.legend {
  display: flex;
  flex-direction: column;
  gap: 5px;
  justify-content: space-between;
  width: fit-content;
  height: fit-content;
}
.legend-item {
  display: flex;
  align-items: center;
  gap: 5px;
}
</style>
