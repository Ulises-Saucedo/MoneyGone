<script setup lang="ts">
import { defineProps } from 'vue'

defineProps({
  income: {
    type: String,
    required: true,
  },
  expenses: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">+${{ income }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">-${{ expenses }}</p>
    </div>
  </div>
</template>
