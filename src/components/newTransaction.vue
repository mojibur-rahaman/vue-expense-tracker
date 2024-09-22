<template>
  <div class="new-transaction w-full">
    <span class="text-md font-medium py-2 text-black">Add New Transaction</span>
    <hr class="h-px my-2 bg-gray-300 border-0" />

    <form id="form" @submit.prevent="onSubmit" class="w-full">
      <div class="mb-2">
        <label for="text" class="block mb-1 text-sm font-medium text-gray-900"
          >Text</label
        >
        <input
          v-model="text"
          type="text"
          id="text"
          class="bg-white border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2"
          placeholder="Describe Transaction..."
        />
      </div>
      <div class="mb-2">
        <label for="amount" class="block mb-1 text-sm font-medium text-gray-900"
          >Amount</label
        >
        <input
          v-model="amount"
          type="text"
          id="amount"
          class="bg-white border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2"
          placeholder="income(+) and expense(-)"
        />
      </div>

      <button
        class="text-white bg-purple-700 hover:bg-purple-800 text-sm w-full px-5 py-2 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
      >
        Add Transaction
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const toast = useToast();
const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    return toast.error("both filds are fillable");
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("transactionSubmitted", transactionData);
  text.value = "";
  amount.value = "";
};
</script>
