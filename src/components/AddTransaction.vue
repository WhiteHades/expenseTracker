<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('')
const toast = useToast()
const emit = defineEmits(['emitTransaction'])

const ssubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both must be filled up')
    return
  }

  const dataTransaction = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('emitTransaction', dataTransaction)

  text.value = ''
  amount.value = ''
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="ssubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
