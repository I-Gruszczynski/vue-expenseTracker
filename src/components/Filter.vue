<template>
  <div class="filter">
    <ul>
      <li
        v-for="(item, index) in items"
        :key="index"
        :class="{ active: activeIndex === index }"
        @click="handleClick(index, item.value)"
      >
        <button>{{ item.name }}</button>
      </li>
    </ul>
    <div class="filter-button">
      <button @click="emit('toggleAddTransaction')">Add +</button>
    </div>
    <div style="clear: both"></div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["filterChange", "toggleAddTransaction"]);

const toggleAddTransaction = () => {
  emit("toggleAddTransaction");
};

const activeIndex = ref(0);
const items = [
  { name: "All", value: "all" },
  { name: "Income", value: "income" },
  { name: "Expense", value: "expense" },
];

const handleClick = (index, value) => {
  activeIndex.value = index;
  emit("filterChange", value);
};

//const props = defineProps({
// transactions: {
//   type: String,
//   required: true,
//   default: "all",
// },
//});
</script>
