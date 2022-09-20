<template>
  <div class='content legal'>
    <LegalPage />
  </div>
</template>

<script setup lang='ts'>
import { defineAsyncComponent, onMounted, watch } from 'vue'
import { useI18n } from 'vue-i18n'
const { locale } = useI18n({ useScope: 'global' })

const LegalPage = defineAsyncComponent(() => import('src/pages/legal/ja-JP/Legal.vue'))

watch(locale, () => {
  // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment, @typescript-eslint/restrict-template-expressions
  LegalPage.value = defineAsyncComponent(() => import(`src/pages/legal/${locale.value}/Legal.vue`))
})

onMounted(() => {
  // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment, @typescript-eslint/restrict-template-expressions
  LegalPage.value = defineAsyncComponent(() => import(`src/pages/legal/${locale.value}/Legal.vue`))
})

</script>
