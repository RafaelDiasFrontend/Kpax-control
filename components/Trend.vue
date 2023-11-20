<script lang="ts" setup>
  interface TrendProps {
    title: string
    amount: number
    lastAmount: number
    color: string
    loading: boolean
  }
  const props = withDefaults(defineProps<TrendProps>(), {
    title: '',
    amount: 0,
    lastAmount: 0,
    color: '',
    loading: false,
  })
  const trendingUp = computed(() => props.amount > props.lastAmount)
  const icon = computed(() =>
    trendingUp.value
      ? 'i-heroicons-arrow-trending-up'
      : 'i-heroicons-arrow-trending-down',
  )

  const { currency } = useCurrency(props.amount)

  const percentageTrend = computed(() => {
    if (props.amount === 0 || props.lastAmount === 0) return '0'

    const bigger = Math.max(props.amount, props.lastAmount)
    const lower = Math.min(props.amount, props.lastAmount)

    const ratio = ((bigger - lower) / lower) * 100
    console.log(bigger, lower, ratio, Math.ceil(ratio))

    return Math.floor(ratio)
  })
</script>

<template>
  <div>
    <div class="font-bold" :class="[color]">{{ title }}</div>
    <div class="text-2x1 font-extrabold text-black dark:text-white mb-2">
      <USkeleton class="h-8 w-full" v-if="loading" />
      <div v-else>{{ currency }}</div>
    </div>

    <div>
      <USkeleton class="h-6 w-full" v-if="loading"></USkeleton>
      <div v-else class="flex space-x-1 items-center text-sm">
        <UIcon
          :name="icon"
          class="w-6 h-6"
          :class="trendingUp ? 'green' : 'red'"
        />
        <div class="text-gray-500 dark:text-gray-400">
          {{ percentageTrend }}% em relação ao ultimo balanço
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .green {
    @apply text-green-600 dark:text-green-400;
  }
  .red {
    @apply text-red-600 dark:text-red-400;
  }
</style>
