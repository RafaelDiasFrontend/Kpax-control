<script setup>
  import { transactionView } from '../constant.ts'
  const selectedView = ref(transactionView.options[1])
</script>

<template>
  <section class="flex items-center justify-between mb-10">
    <h1 class="text-4x1 font-extrabold">Summary</h1>
    <USelect :options="transactionView.options" v-model="selectedView">
    </USelect>
  </section>
</template>
