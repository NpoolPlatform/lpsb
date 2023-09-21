<template>
  <AleoPage />
</template>

<script setup lang='ts'>

import { defineAsyncComponent } from 'vue'

const AleoPage = defineAsyncComponent(() => import('src/components/product/Aleo.vue'))

</script>
