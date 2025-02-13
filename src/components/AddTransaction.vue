<script setup lang="ts">
import { reactive, defineEmits } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const form = reactive({
  text: '',
  amount: 0,
})
const emit = defineEmits(['onTransaction'])

function onSubmit() {
  if (!form.text || !form.amount) {
    toast.error('Please fill in both text and amount fields')
    return
  }

  const transactionData = {
    text: form.text,
    amount: parseFloat(`${form.amount}`),
  }

  emit('onTransaction', transactionData)

  form.text = ''
  form.amount = 0
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="form.text" />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount <br />
        (negative - expense, positive - income)
      </label>
      <input type="number" id="amount" placeholder="Enter amount..." v-model="form.amount" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
