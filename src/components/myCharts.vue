<template>
  <div class="w-1/2 mx-auto">
    <!-- Render Pie Chart with dynamic data -->
    <Pie :data="data" :options="options" />
  </div>
</template>

<script setup>
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from "chart.js";
import { Pie } from "vue-chartjs";

ChartJS.register(ArcElement, Tooltip, Legend);

// Options for the chart
const data = {
  labels: ["VueJs", "EmberJs", "ReactJs", "AngularJs"],
  datasets: [
    {
      backgroundColor: ["#41B883", "#E46651", "#00D8FF", "#DD1B16"],
      data: [40, 20, 80, 10],
    },
  ],
};
const options = {
  responsive: true,
};
</script>
