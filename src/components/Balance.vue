<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <h4>Your Available Balance</h4>

  <!-- <h2 v-if="!check" id="negative_Balance">${{ total.toLocaleString() }}</h2> -->
  <h2 id="balance">${{ total.toLocaleString() }}</h2>
</template>
