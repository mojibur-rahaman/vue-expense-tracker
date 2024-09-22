<template>
  <div class="history w-full my-4">
    <span class="text-md font-medium py-2 text-black">History</span>
    <hr class="h-px my-2 bg-gray-300 border-0" />
    <ul class="historyItmes">
      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        class="relative group flex flex-row justify-between w-full h-auto pl-4 hover:pl-10 py-2 mb-2 bg-white shadow-md border-r-4"
        :class="transaction.amount > 0 ? 'border-green-600' : 'border-red-600'"
      >
        <p>
          {{ transaction.text }}
          <button
            @click="deleteTransaction(transaction.id)"
            class="absolute left-2 top-2 bg-red-500 px-2 rounded-sm hidden group-hover:block group-hover:delay-300 text-white"
          >
            x
          </button>
        </p>
        <p class="pr-4 text-gray-600">${{ transaction.amount }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
const props = defineProps({
  transactions: {
    type: Array,
    Required: true,
  },
});

const emit = defineEmits(["transactionDeleted"]);
const deleteTransaction = (id) => {
  emit("transactionDeleted", id);
};
</script>
