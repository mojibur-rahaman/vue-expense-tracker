<template>
  <div class="income-expense text-center w-full">
    <div class="grid grid-cols-2 py-3 bg-white shadow-md">
      <div class="1">
        <p class="text-md font-medium py-2 text-black">Income</p>
        <p class="text-md text-xl font-medium text-green-500">${{ income }}</p>
      </div>
      <div class="1">
        <p class="text-md font-medium py-2 text-black">expense</p>
        <p class="text-md text-xl font-medium text-red-500">${{ expenses }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
const props = defineProps({
  income: {
    type: Number,
    required: true,
  },
  expenses: {
    type: Number,
    required: true,
  },
});
</script>
