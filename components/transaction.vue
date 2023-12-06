<script setup>
const props = defineProps({
  transaction: Object,
})

// VERIFY IF IT IS A INCOME OR A EXPENSE
const isIncoming = computed(() => props.transaction.type === 'Entrada')

const icon = computed(() =>
  isIncoming.value
    ? 'i-heroicons-arrow-up-right'
    : 'i-heroicons-arrow-down-left'
)

const iconColor = computed(() =>
  isIncoming.value ? 'text-green-600' : 'text-red-600'
)

const { currency } = useCurrency(props.transaction.amount)

const isLoading = ref(false)
const supabase = useSupabaseClient()
const toast = useToast()

const deleteTransaction = async () => {
  isLoading.value = true

  try {
    await supabase.from('Transactions').delete().eq('id', props.transaction.id)
    toast.add({
      title: 'Transação excluida com sucesso!',
      icon: 'i-heroicons-check-circle',
      color: 'green',
    })
  } catch (error) {
    toast.add({
      title: 'Ops Ocorreu um erro!',
      icon: 'i-heroicons-exclamation-circle',
      color: 'red',
    })
  } finally {
    isLoading.value = false
  }
}
const items = [
  [
    {
      label: 'Editar',
      icon: 'i-heroicons-pencil-square-20-solid',
      click: () => console.log('Editado'),
    },
    {
      label: 'Delete',
      icon: 'i-heroicons-trash-20-solid',
      click: deleteTransaction,
    },
  ],
]
</script>

<template>
  <div
    class="grid grid-cols-2 py-4 border-b border-gray-200 dark:border-gray-800 text-gray-900 dark:text-gray-100 font-bold"
  >
    <div class="flex items-center justify-between">
      <div class="flex items-center space-x-1">
        <UIcon :name="icon" :class="iconColor" />
        <div>{{ transaction.description }}</div>
      </div>
      <div>
        <UBadge color="white" v-if="transaction.category">{{
          transaction.category
        }}</UBadge>
      </div>
    </div>

    <div class="flex items-center justify-end space-x-2">
      <div>{{ currency }}</div>
      <div>
        <UDropdown :items="items" :popper="{ placement: 'bottom-end' }">
          <UButton
            color="white"
            variant="ghost"
            trailing-icon="i-heroicons-ellipsis-horizontal"
            :loading="isLoading"
          />
        </UDropdown>
      </div>
    </div>
  </div>
</template>
