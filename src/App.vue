<template>
  <main>
    <div class="w-10/12 md:w-1/2 lg:w-1/4 mx-auto h-screen my-4">
      <div class="wrapper w-full">
        <headers />

        <div v-if="transactions.length > 0" class="w-2/3 mx-auto">
          <Doughnut :data="chartData" :options="options" />
        </div>
        <amount :total="+total" />
        <incomeExpense :income="+income" :expenses="+expenses" />
        <transaction
          :transactions="transactions"
          @transactionDeleted="handleTransactionDeleted"
        />
        <newTransaction @transactionSubmitted="handleTransactionSubmitted" />
      </div>
    </div>
  </main>
</template>
<script setup>
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";
import headers from "./components/headers.vue";
import amount from "./components/amount.vue";
import incomeExpense from "./components/incomeExpense.vue";
import transaction from "./components/transaction.vue";
import newTransaction from "./components/newTransaction.vue";

import { Chart as ChartJS, ArcElement, Tooltip, Legend } from "chart.js";
import { Doughnut } from "vue-chartjs";
ChartJS.register(ArcElement, Tooltip, Legend);
const toast = useToast();
const transactions = ref([]);

// On Loading get transaction form localstorage
onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

// Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});
// Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
// Get Expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Add New Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionToLocalStorage();

  return toast.success("Transaction Sussfully Add");
};

// Generate Unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000);
};

// Delete Transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionToLocalStorage();
  toast.success("Delete Transaction Successfully");
};

// Save transaction on loaclstorage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
// Chart js
// Daynamic Data
const chartData = computed(() => {
  return {
    labels: ["income", "expenses"],
    datasets: [
      {
        backgroundColor: ["#41B883", "#DD1B16"],
        data: [income.value, expenses.value],
      },
    ],
  };
});

// chart Options
const options = {
  responsive: true,
};
</script>

<!-- <template>
  <main>
    <div class="w-10/12 md:w-1/2 lg:w-1/4 mx-auto h-screen my-4">
      <div class="wrapper w-full">
        <headers />
        <pie :data="chartData" :options="options" />
        <amount :total="+total" />
        <incomeExpense :income="+income" :expenses="+expenses" />
        <transaction
          :transactions="transactions"
          @transactionDeleted="handleTransactionDeleted"
        />
        <newTransaction @transactionSubmitted="handleTransactionSubmitted" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";
import headers from "./components/headers.vue";
import amount from "./components/amount.vue";
import incomeExpense from "./components/incomeExpense.vue";
import transaction from "./components/transaction.vue";
import newTransaction from "./components/newTransaction.vue";
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from "chart.js";
import { Pie } from "vue-chartjs";

ChartJS.register(ArcElement, Tooltip, Legend);

const toast = useToast();
const transactions = ref([]);

// On Loading get transaction from localstorage
onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

// Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Get Expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Computed property for chart data
const chartData = computed(() => {
  return {
    labels: ["Income", "Expense"],
    datasets: [
      {
        backgroundColor: ["#41B883", "#DD1B16"],
        data: [income.value, expenses.value],
      },
    ],
  };
});

// Chart options
const options = {
  responsive: true,
};

// Add New Transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionToLocalStorage();
  return toast.success("Transaction Successfully Added");
};

// Generate Unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000);
};

// Delete Transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionToLocalStorage();
  toast.success("Transaction Deleted Successfully");
};

// Save transaction in localstorage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script> -->
