<template>
  <v-btn class="theme-btn" @click="toggleTheme">toggle theme</v-btn>
</template>
<script>
import { useTheme } from "vuetify";

export default {
  setup() {
    const theme = useTheme();

    function toggleTheme() {
      theme.global.name.value = theme.global.current.value.dark
        ? "light"
        : "dark";
    }

    return {
      theme,
      toggleTheme,
    };
  },
};
</script>

<style scoped>
.theme-btn {
  position: absolute;
  top: 0px;
  right: 0px;
}
</style>
