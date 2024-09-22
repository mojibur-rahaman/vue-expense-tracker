<template>
  <div class="flex flex-row justify-between">
    <div class="amount w-full mb-4">
      <p class="text-md font-medium text-black">your balance</p>
      <p class="text-3xl font-semibold text-black">${{ total }}</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>
