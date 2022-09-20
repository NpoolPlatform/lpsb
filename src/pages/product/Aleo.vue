<template>
  <AleoPage />
</template>

<script setup lang='ts'>

import { defineAsyncComponent, onMounted, watch } from 'vue'
import { useI18n } from 'vue-i18n'
const { locale } = useI18n({ useScope: 'global' })

const AleoPage = defineAsyncComponent(() => import('src/components/product/ja-JP/Aleo.vue'))

watch(locale, () => {
  // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment, @typescript-eslint/restrict-template-expressions
  AleoPage.value = defineAsyncComponent(() => import(`src/components/product/${locale.value}/Aleo.vue`))
})

onMounted(() => {
  // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment, @typescript-eslint/restrict-template-expressions
  AleoPage.value = defineAsyncComponent(() => import(`src/components/product/${locale.value}/Aleo.vue`))
})
</script>
