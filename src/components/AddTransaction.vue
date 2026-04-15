<template>
  <div class="addTransaction">
    <h3>Add new transaction</h3>
    <hr />
    <div class="addTransaction-content">
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text"><h4>Text</h4></label>
          <input
            type="text"
            id="text"
            placeholder="Enter text..."
            v-model="text"
          />
        </div>
        <div class="form-control">
          <label for="amount"><h4>Amount</h4></label>
          <input
            type="text"
            id="amount"
            placeholder="Enter amount..."
            v-model="amount"
          />
        </div>
        <h4 class="addTransaction-productHeader">Product</h4>
        <div class="addTransaction-product">
          <div class="addTransaction-product-food">
            <div
              class="addTransaction-product-food-icon"
              :class="{ active: productType === 'Food' }"
              @click="productType = 'Food'"
            >
              <Icon
                icon="mdi:food"
                width="40"
                height="40"
                :style="{ color: productType === 'Food' ? '#fff' : '#66eb5e' }"
              />
            </div>
            Food
          </div>
          <div class="addTransaction-product-transport">
            <div
              class="addTransaction-product-transport-icon"
              :class="{ active: productType === 'Transport' }"
              @click="productType = 'Transport'"
            >
              <Icon
                icon="mdi:car"
                width="40"
                height="40"
                :style="{
                  color: productType === 'Transport' ? '#fff' : '#103bdc',
                }"
              />
            </div>
            Transport
          </div>
          <div class="addTransaction-product-services">
            <div
              class="addTransaction-product-services-icon"
              :class="{ active: productType === 'Services' }"
              @click="productType = 'Services'"
            >
              <Icon
                icon="ri:service-line"
                width="40"
                height="40"
                :style="{
                  color: productType === 'Services' ? '#fff' : '#d7e72c',
                }"
              />
            </div>
            Services
          </div>
          <div class="addTransaction-product-clothing">
            <div
              class="addTransaction-product-clothing-icon"
              :class="{ active: productType === 'Clothing' }"
              @click="productType = 'Clothing'"
            >
              <Icon
                icon="lsicon:clothes-filled"
                width="40"
                height="40"
                :style="{
                  color: productType === 'Clothing' ? '#fff' : '#f51313',
                }"
              />
            </div>
            Clothing
          </div>
          <div class="addTransaction-product-others">
            <div
              class="addTransaction-product-others-icon"
              :class="{ active: productType === 'Others' }"
              @click="productType = 'Others'"
            >
              <Icon
                icon="mdi:help-circle"
                width="40"
                height="40"
                :style="{
                  color: productType === 'Others' ? '#fff' : '#fa00f0',
                }"
              />
            </div>
            Others
          </div>
        </div>
        <h4>Type</h4>
        <div class="addTransaction-type">
          <div
            class="addTransaction-type-expense"
            :class="{ active: transactionType === 'expense' }"
            @click="transactionType = 'expense'"
          >
            Expense
          </div>
          <div
            class="addTransaction-type-income"
            :class="{ active: transactionType === 'income' }"
            @click="transactionType = 'income'"
          >
            Income
          </div>
        </div>
        <button class="addTransaction-btn">Add transaction</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { useToast } from "vue-toastification";
import { ref } from "vue";

import { Icon } from "@iconify/vue";
const text = ref("");
const amount = ref("");

const transactionType = ref("expense");
const productType = ref("others");

// Get toast interface
const toast = useToast();
const showAddModal = ref(false);

const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    // Display a toast error message if either field is empty
    toast.error("Both fields must be filled.");
    return;
  }

  let finalAmount = parseFloat(amount.value);

  if (isNaN(finalAmount)) {
    toast.error("Amount must be a valid number.");
    return;
  }

  if (transactionType.value === "expense" && finalAmount > 0) {
    finalAmount = -finalAmount; // Convert to negative for expenses
  } else if (transactionType.value === "income" && finalAmount < 0) {
    finalAmount = -finalAmount; // Convert to positive for income
  }

  const transactionData = {
    text: text.value,
    amount: finalAmount,
    type: transactionType.value,
    product: productType.value,
  };

  console.log("Submitting transaction:", transactionData);

  emit("closeModal");
  emit("transactionSubmitted", transactionData);
  // Clear form fields
  text.value = "";
  amount.value = "";
  transactionType.value = "expense";
  productType.value = "Others";
};
</script>
