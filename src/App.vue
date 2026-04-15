<template>
  <HeaderTracker />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <Filter
      @toggleAddTransaction="showAddModal = true"
      @filterChange="handleFilterChange"
    />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
      :filter="filter"
    />
    <div class="modal-addTransaction" v-if="showAddModal">
      <div class="modal-content">
        <button class="modal-content-close-btn" @click="showAddModal = false">
          x
        </button>
        <AddTransaction
          @transactionSubmitted="handleTransactionSubmitted"
          @closeModal="showAddModal = false"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import HeaderTracker from "./components/HeaderTracker.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import Filter from "./components/Filter.vue";

import { ref, computed, onMounted } from "vue";

import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

const showAddModal = ref(false);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Submit transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
    product: transactionData.product,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction added.");
};

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

// Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id,
  );

  saveTransactionsToLocalStorage();

  toast.success("Transaction deleted.");
};

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

// Dodaj stan filtra
const filter = ref("all"); // 'all', 'income', 'expense'

// Obsługa zmiany filtra
const handleFilterChange = (type) => {
  filter.value = type;
};
</script>
