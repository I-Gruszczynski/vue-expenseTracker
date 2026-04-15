<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in filteredTransactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      <div class="transaction-info">
        <p>{{ transaction.text }}</p>

        <p :class="productClass[transaction.product]">
          {{ transaction.product }}
        </p>
      </div>
      <p>${{ transaction.amount }}</p>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
import { computed } from "vue";
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
  filter: {
    type: String,
    required: true,
  },
});

const filteredTransactions = computed(() => {
  if (props.filter === "all") return props.transactions;
  else if (props.filter === "income") {
    return props.transactions.filter((t) => t.amount > 0);
  } else if (props.filter === "expense") {
    return props.transactions.filter((t) => t.amount < 0);
  }
  return props.transactions;
});

const emit = defineEmits(["transactionDeleted"]);

const deleteTransaction = (id) => {
  emit("transactionDeleted", id);
};

const productClass = {
  food: "product-food",
  transport: "product-transport",
  services: "product-services",
  clothing: "product-clothes",
  others: "product-others",
};
</script>

<style scoped>
.product-food {
  color: #66eb5e;
}
.product-transport {
  color: #103bdc;
}
.product-services {
  color: #d7e72c;
}
.product-clothes {
  color: #f51313;
}
.product-others {
  color: #fa00f0;
}
</style>
