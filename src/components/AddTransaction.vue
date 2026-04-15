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
            <div class="addTransaction-product-food-icon">
              <Icon
                icon="mdi:food"
                width="40"
                height="40"
                style="color: #66eb5e"
              />
            </div>
            Food
          </div>
          <div class="addTransaction-product-transport">
            <div class="addTransaction-product-transport-icon">
              <Icon
                icon="mdi:car"
                width="40"
                height="40"
                style="color: #103bdc"
              />
            </div>
            Transport
          </div>
          <div class="addTransaction-product-services">
            <div class="addTransaction-product-services-icon">
              <Icon
                icon="ri:service-line"
                width="40"
                height="40"
                style="color: #d7e72c"
              />
            </div>
            Services
          </div>
          <div class="addTransaction-product-clothing">
            <div class="addTransaction-product-clothing-icon">
              <Icon
                icon="lsicon:clothes-filled"
                width="40"
                height="40"
                style="color: #f51313"
              />
            </div>
            Clothing
          </div>
          <div class="addTransaction-product-others">
            <div class="addTransaction-product-others-icon">
              <Icon
                icon="mdi:help-circle"
                width="40"
                height="40"
                style="color: #fa00f0"
              />
            </div>
            Others
          </div>
        </div>
        <h4>Type</h4>
        <div class="addTransaction-type">
          <div class="addTransaction-type-income">Income</div>
          <div class="addTransaction-type-expense">Expense</div>
        </div>
        <button class="btn">Add transaction</button>
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

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };
  emit("closeModal");
  emit("transactionSubmitted", transactionData);
  // Clear form fields
  text.value = "";
  amount.value = "";
};
</script>
