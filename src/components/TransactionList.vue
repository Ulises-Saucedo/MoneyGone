<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'
import type { Transaction } from '@/types'

defineProps({
  transactions: {
    type: Array<Transaction>,
    required: true,
  },
})

const emit = defineEmits(['onDeleteTransaction'])

function deleteTransaction(id: string) {
  emit('onDeleteTransaction', id)
}
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
      :key="transaction.id"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>
