<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const emit = defineEmits(['transactionSubmitted'])
const text = ref('')
const amount = ref('')

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('All fields must be filled')
    return
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData)
  console.log(amount.value)
  text.value = ''
  amount.value = ''
  date.value = ''
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form @submit.prevent="onSubmit" id="form">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (Add negative(-) sign for expenses )</label
      >

      <input v-model="amount" type="number" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
