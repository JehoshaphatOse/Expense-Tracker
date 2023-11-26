<script setup>
import { ref, computed, onMounted } from 'vue'
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])
const viewAll = ref(false)
const showAllTransaction = ref([])

const toggleViewAll = () => {
  console.log('clicked')
  // viewAll.value = !viewAll.value
  // updateDisplayedTransactions()
}
// const updateDisplayedTransactions = () => {
//   if (viewAll.value) {
//     showAllTransaction.value = transactions.value
//   } else {
//     Limit the number of displayed items, e.g., show the first 3 items
//     showAllTransaction.value = transactions.value.slice(0, 3)
//   }
// }
// check for saved transaction in local storage
onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem(transactions))
  if (savedTransaction) {
    transactions.value = savedTransaction
  }
})

//Get Balance
const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + transactions.amount
  }, 0)
})

//Get income

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount
    }, 0)
    .toFixed(2)
})

const check = () => {
  if (total.value <= 0) {
    return total.value
  }
}

//Get expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount
    }, 0)
    .toFixed(2)
})

//Add Transaction
const handleSubmittedTransaction = (transactionData) => {
  console.log('clicked')
  transactions.value.push({
    id: Math.floor(Math.random() * 10000),
    text: transactionData.text,
    amount: transactionData.amount
  })
  saveTransaction()
  toast.success('Transaction Added Succesfully')
}

console.log(total)

//Delete transaction

const handleDeletedTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  saveTransaction()
  toast.success('Transaction Deleted Succesfully')
}

//store transaction to localstorage

const saveTransaction = () => {
  localStorage.setItem(transactions, JSON.stringify(transactions.value))
}
</script>
<template>
  <Header />
  <div class="container">
    <div class="header-container">
      <div>
        <Balance :total="+total" :check="check" />
        <IncomeExpenses :income="+income" :expense="+expense" />
      </div>
      <div>
        <AddTransaction @transactionSubmitted="handleSubmittedTransaction" />
      </div>
    </div>

    <TransactionList
      :transactions="transactions"
      @deletedTransaction="handleDeletedTransaction"
      @viewAllHistory="toggleViewAll"
    />
  </div>
</template>
