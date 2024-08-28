<template>
  <h2>{{ $t('MSG_MINING_DASHBOARD') }}</h2>
  <MiningCard
    v-for='goodProfit in goodProfits'
    :key='goodProfit.CoinTypeID'
    :profit='goodProfit'
  />
  <!-- <SpacemeshMockCard /> -->
</template>

<script setup lang='ts'>
import { computed, defineAsyncComponent } from 'vue'
import { appcoin, coincurrency, utils, sdk, apppowerrental } from 'src/npoolstore'
import { MyGoodProfit } from 'src/localstore/ledger/types'

const MiningCard = defineAsyncComponent(() => import('src/components/dashboard/MiningCard.vue'))
// const SpacemeshMockCard = defineAsyncComponent(() => import('src/components/dashboard/SpacemeshMockCard.vue'))

const currency = coincurrency.useCurrencyStore()
const coin = appcoin.useAppCoinStore()

const getTBD = computed(() => (appGoodID: string) => {
  return sdk.appPowerRental.description(appGoodID, 5)
})

const coinCurrency = (coinTypeID: string) => {
  return (currency.currency(coinTypeID) || coin.localCurrency(undefined, coinTypeID))
}

const goodProfits = computed(() => Array.from(sdk.ledgerProfit.goodProfits(utils.IntervalKey.All)).map((el) => {
  const _good = sdk.appPowerRental.appPowerRental(el.AppGoodID)as apppowerrental.AppPowerRental
  const now = Math.floor(Date.now() / 1000)
  const remain = now - _good?.ServiceStartAt >= 0 ? now - _good?.ServiceStartAt : 0
  const daysMined = Math.floor(remain / 24 / 60 / 60)
  const durationDays = Number((_good?.MinOrderDurationSeconds || 0) / utils.SecondsPerDay)
  const daysRemaining = durationDays - daysMined > 0 ? durationDays - daysMined : 0
  return {
    ...el,
    Units: el.Units,
    CoinPreSale: coin.preSale(undefined, el.CoinTypeID),
    TotalInComing: sdk.ledgerProfit.totalIncoming(utils.IntervalKey.All, el.CoinTypeID, el.AppGoodID),
    TotalUSDInComing: coinCurrency(el.CoinTypeID) * sdk.ledgerProfit.totalIncoming(utils.IntervalKey.All, el.CoinTypeID, el.AppGoodID),
    Last24HoursInComing: sdk.ledgerProfit.totalIncoming(utils.IntervalKey.LastDay, el.CoinTypeID, el.AppGoodID),
    Last24HoursUSDInComing: coinCurrency(el.CoinTypeID) * sdk.ledgerProfit.totalIncoming(utils.IntervalKey.LastDay, el.CoinTypeID, el.AppGoodID),
    Last30DaysInComing: sdk.ledgerProfit.totalIncoming(utils.IntervalKey.LastMonth, el.CoinTypeID, el.AppGoodID),
    Last30DaysUSDInComing: coinCurrency(el.CoinTypeID) * sdk.ledgerProfit.totalIncoming(utils.IntervalKey.LastMonth, el.CoinTypeID, el.AppGoodID),
    GoodSaleEndAt: _good?.SaleEndAt,
    TotalEstimatedDailyReward: Number(el.Units) * parseFloat(good.good(undefined, el.AppGoodID)?.LastUnitRewardAmount as string),
    MiningStartDate: _good?.ServiceStartAt > Math.ceil(Date.now() / 1000) ? getTBD.value(el.AppGoodID) : utils.formatTime(_good?.ServiceStartAt, 'YYYY-MM-DD'),
    DaysMined: daysMined > durationDays ? durationDays : daysMined,
    DaysRemaining: daysRemaining,
    GoodServicePeriodDays: durationDays
  } as MyGoodProfit
}).sort((a, b) => {
  if (a.CoinUnit.localeCompare(b.CoinUnit, 'zh-CN')) {
    return a.CoinUnit.localeCompare(b.CoinUnit, 'zh-CN')
  } else {
    return a.GoodSaleEndAt > b.GoodSaleEndAt ? -1 : 1
  }
}))
</script>
