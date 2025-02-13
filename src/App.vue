<script setup lang="ts">
import { onMounted, ref, type Ref, computed } from 'vue'
import type { Transaction } from '@/types'
import { useToast } from 'vue-toastification'
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

const toast = useToast()
const transactions: Ref<Transaction[]> = ref([])

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
})

function onTransaction(data: { text: string; amount: number }) {
  transactions.value.push({
    id: crypto.randomUUID(),
    ...data,
  })

  saveTransactionsToLocalStorage()

  toast.success('Transaction added successfully!')
}

function onDeleteTransaction(id: string) {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  saveTransactionsToLocalStorage()

  toast.success('Transaction deleted successfully!')
}

function saveTransactionsToLocalStorage() {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})
</script>

<template>
  <Header />

  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" @on-delete-transaction="onDeleteTransaction" />
    <AddTransaction @on-transaction="onTransaction" />
  </div>
</template>
