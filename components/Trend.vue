<script lang="ts" setup>
  interface TrendProps {
    title: string
    amount: number
    lastAmount: number
    color: string
    loading: boolean
  }
  withDefaults(defineProps<TrendProps>(), {
    title: '',
    amount: 0,
    lastAmount: 0,
    color: '',
    loading: false,
  })
</script>

<template>
  <div>
    <div class="font-bold" :class="[color]">{{ title }}</div>
    <div class="text-2x1 font-extrabold text-black dark:text-white mb-2">
      <USkeleton class="h-8 w-full" v-if="loading" />
      <div v-else>{{ amount }}</div>
    </div>

    <div>
      <USkeleton class="h-6 w-full" v-if="loading"></USkeleton>
      <div v-else class="flex space-x-1 items-center text-sm">
        <UIcon
          name="i-heroicons-arrow-trending-up"
          class="w-6 h-6"
          :class="[color]"
        />
        <div class="text-gray-500 dark:text-gray-400">30%</div>
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
