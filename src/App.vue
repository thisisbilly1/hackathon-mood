<template>
  <v-app>
    <ThemeButton />
    <PolarGraph :moods="moods"/>
    <LineGraph :moods="moods"/>
  </v-app>
</template>
<script>
import PolarGraph from './components/PolarGraph.vue';
import LineGraph from './components/LineGraph.vue';
import ThemeButton from './components/ThemeButton.vue';

export default {
  components: { PolarGraph, LineGraph, ThemeButton },
  setup() {
    function getBackgroundColor(color, alpha = 0.7) {
      const opacity = Math.floor(alpha * 255);
      return color + opacity.toString(16).toUpperCase();
    }

    const moods = [
      {
        label: "healthy",
        color: "#07B151",
        backgroundColor: getBackgroundColor("#07B151"),
      },
      {
        label: "coping",
        color: "#FCED23",
        backgroundColor: getBackgroundColor("#FCED23"),
      },
      {
        label: "struggling",
        color: "#F6851E",
        backgroundColor: getBackgroundColor("#F6851E"),
      },
      {
        label: "unwell",
        color: "#D52127",
        backgroundColor: getBackgroundColor("#D52127"),
      },
      {
        label: "depressed",
        color: "#733B97",
        backgroundColor: getBackgroundColor("#733B97"),
      },
    ];
  
    return {
      moods
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
  margin-top: 50px;
}
</style>
