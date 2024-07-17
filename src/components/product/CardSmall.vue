<template>
  <div class='product'>
    <div class='product-heading'>
      <img class='icon' :src='sdk.appPowerRental.mainCoinLogo(appGoodId)'>
      <h3 class='product-title'>
        {{ sdk.appPowerRental.displayName(appGoodId, 0) }}
      </h3>
    </div>
    <h4 class='price'>
      <span>{{ sdk.appPowerRental.unitPrice(appGoodId) }}</span> {{ constant.PriceCoinName }} / {{ $t(good?.QuantityUnit || '') }}
    </h4>
    <div class='line'>
      <span class='label'>{{ $t('MSG_DAILY_MINING_REWARDS') }}:</span>
      <span class='value'>*{{ sdk.appPowerRental.mainCoinUnit(appGoodId) }} / {{ $t('MSG_DAY') }}</span>
    </div>

    <div class='line'>
      <span class='label'>{{ $t('MSG_SERVICE_PERIOD') }}:</span>
      <span class='value'>{{ sdk.appPowerRental.minOrderDurationDays(appGoodId) }} {{ $t('MSG_DAYS') }}</span>
    </div>

    <div class='line'>
      <span class='label'>{{ $t('MSG_TECHNIQUE_SERVICE_FEE') }}:</span>
      <span class='value'>20%</span>
    </div>

    <!-- div class='line'>
      <span class='label'>{{ $t('MSG_MAINTENANCE_FEE') }}:</span>
      <span class='value'>* / {{ $t('MSG_DAY') }}</span>
    </div -->

    <div class='line'>
      <span class='label'>{{ $t('MSG_ORDER_EFFECTIVE') }}:</span>
      <span class='value'>{{ utils.formatTime(good?.ServiceStartAt as number, undefined) }}</span>
    </div>
    <button class='alt' @click='onPurchaseClick'>
      {{ $t('MSG_PURCHASE') }}
    </button>
  </div>
</template>

<script setup lang='ts'>
import { defineProps, toRef, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { appcoin, utils, constant, sdk } from 'src/npoolstore'
import { getCoins } from 'src/api/chain'

interface Props {
  appGoodId: string
}

const props = defineProps<Props>()
const appGoodId = toRef(props, 'appGoodId')
const good = computed(() => sdk.appPowerRental.appPowerRental(appGoodId.value))

const coin = appcoin.useAppCoinStore()
const productInfo = computed(() => coin.productPage(undefined, sdk.appPowerRental.mainCoinTypeID(appGoodId.value) as string))

const router = useRouter()
const onPurchaseClick = () => {
  let target = '/purchase'
  if (productInfo.value?.length) {
    target = productInfo.value
  }

  void router.push({
    path: target,
    query: {
      appGoodID: appGoodId.value
    }
  })
}

onMounted(() => {
  if (!coin.coins(undefined).length) {
    getCoins(0, 100)
  }
})

</script>
