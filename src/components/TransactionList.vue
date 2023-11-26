<script setup>
import { defineProps } from 'vue'

const emit = defineEmits(['deletedTransaction', 'viewHistory'])
const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const deleteTransaction = (id) => {
  emit('deletedTransaction', id)
}
const viewAllHistory = () => {
  emit('viewHistory')
}
</script>

<template>
  <h3>History</h3>
  <!-- <p @click="viewAllHistory()">View All</p> -->
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}<span>&#x20A6;{{ Math.abs(transaction.amount).toLocaleString() }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
    </li>

    <!-- <li class="plus">Pay <span>$400</span><button class="delete-btn">x</button></li> -->
  </ul>
</template>
