<script setup>
  import { transactionView } from '../constant.ts'
  const selectedView = ref(transactionView.options[1])

  const supabase = useSupabaseClient()

  const transactions = ref([])

  const { data, pending } = await useAsyncData('transactions', async () => {
    const { data, error } = await supabase.from('Transactions').select()
    if (error) return []
    return data
  })
  transactions.value = data.value
</script>

<template>
  <section class="flex items-center justify-between mb-10">
    <h1 class="text-4x1 font-extrabold">Summary</h1>
    <USelect :options="transactionView.options" v-model="selectedView">
    </USelect>
  </section>
  <section
    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 sm:gap-16 mb-10"
  >
    <Trend
      color="green"
      title="Income"
      :amount="4000"
      :lastAmount="4000"
      :loading="false"
    />
    <Trend
      color="red"
      title="Income"
      :amount="4000"
      :lastAmount="5000"
      :loading="false"
    />
    <Trend
      color="green"
      title="Income"
      :amount="4000"
      :lastAmount="3000"
      :loading="false"
    />
    <Trend
      color="red"
      title="Income"
      :amount="4000"
      :lastAmount="4100"
      :loading="false"
    />
  </section>

  <section>
    <Transaction
      v-for="transaction in transactions"
      :key="transaction.id"
      :transaction="transaction"
    />
  </section>
</template>
