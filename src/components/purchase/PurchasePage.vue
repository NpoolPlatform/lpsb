<template>
  <BackPage>
    <div class='content order-page'>
      <div class='product-container content-glass'>
        <div class='product-title-section fallback-title'>
          <div class='product-page-icon'>
            <img :src='sdk.appPowerRental.mainCoinLogo(appGoodId)'>
          </div>
          <h1>{{ sdk.appPowerRental.mainCoinName(appGoodId) }} {{ $t('MSG_MINING') }}</h1>
        </div>
        <slot />
      </div>
      <div class='hr' />
    </div>
  </BackPage>
</template>

<script setup lang='ts'>
import { sdk } from 'src/npoolstore'
import { defineAsyncComponent, defineProps, toRef } from 'vue'

interface Props {
  appGoodId: string
}

const props = defineProps<Props>()
const appGoodId = toRef(props, 'appGoodId')

const BackPage = defineAsyncComponent(() => import('src/components/page/BackPage.vue'))

</script>

<style lang='sass' scoped>
.fallback-title
  width: 100%
</style>
