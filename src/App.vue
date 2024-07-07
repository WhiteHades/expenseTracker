<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + Math.abs(transaction.amount)
    }, 0)
    .toFixed(2)
})

const handleSubmitTransaction = (dataTransaction) => {
  transactions.value.push({
    id: uniqeID(),
    text: dataTransaction.text,
    amount: dataTransaction.amount
  })
  savedTransactionsToLocalStorage();
  toast.success('Added transaction');
}

const uniqeID = () => {
  return Math.floor(Math.random() * 100)
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  savedTransactionsToLocalStorage();
  toast.success('Deleted transaction')
}

const savedTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @deletedTransaction="handleDeleteTransaction" />
    <AddTransaction @emitTransaction="handleSubmitTransaction" />
  </div>
</template>
