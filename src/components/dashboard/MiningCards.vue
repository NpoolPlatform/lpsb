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
import { appgood, appcoin, coincurrency, ledgerprofit, user, utils, goodbase } from 'src/npoolstore'
import { IntervalKey } from 'src/const/const'
import { MyGoodProfit } from 'src/localstore/ledger/types'

const MiningCard = defineAsyncComponent(() => import('src/components/dashboard/MiningCard.vue'))
// const SpacemeshMockCard = defineAsyncComponent(() => import('src/components/dashboard/SpacemeshMockCard.vue'))

const currency = coincurrency.useCurrencyStore()
const coin = appcoin.useAppCoinStore()
const logined = user.useLocalUserStore()

const good = appgood.useAppGoodStore()

const getTBD = computed(() => (appGoodID: string) => {
  const _good = good.good(undefined, appGoodID)
  return _good?.Descriptions?.[5] || '*'
})

const profit = ledgerprofit.useProfitStore()
const goodProfits = computed(() => Array.from(profit.goodProfits(undefined, logined.loginedUserID)).map((el) => {
  const _good = good.good(undefined, el.AppGoodID) as appgood.Good
  const now = Math.floor(Date.now() / 1000)
  const remain = now - _good?.ServiceStartAt >= 0 ? now - _good?.ServiceStartAt : 0
  const daysMined = Math.floor(remain / 24 / 60 / 60)
  let durationDays = Number(el.MaxOrderDuration)
  switch (el.DurationType) {
    case goodbase.GoodDurationType.GoodDurationByHour:
      durationDays = durationDays / 24
      break
    case goodbase.GoodDurationType.GoodDurationByDay:
      break
    case goodbase.GoodDurationType.GoodDurationByMonth:
      durationDays = durationDays * 30
      break
    case goodbase.GoodDurationType.GoodDurationByYear:
      durationDays = durationDays * 365
      break
  }

  const daysRemaining = durationDays - daysMined > 0 ? durationDays - daysMined : 0
  return {
    ...el,
    Units: el.Units,
    CoinPreSale: coin.preSale(undefined, el.CoinTypeID),
    TotalInComing: profit.totalIncoming(undefined, logined.loginedUserID, el.CoinTypeID, el.AppGoodID),
    TotalUSDInComing: currency.currency(el.CoinTypeID) * profit.totalIncoming(undefined, logined.loginedUserID, el.CoinTypeID, el.AppGoodID),
    Last24HoursInComing: profit.intervalGoodIncoming(undefined, logined.loginedUserID, IntervalKey.LastDay, el.CoinTypeID, el.AppGoodID),
    Last24HoursUSDInComing: currency.currency(el.CoinTypeID) * profit.intervalGoodIncoming(undefined, logined.loginedUserID, IntervalKey.LastDay, el.CoinTypeID, el.AppGoodID),
    Last30DaysInComing: profit.intervalGoodIncoming(undefined, logined.loginedUserID, IntervalKey.LastMonth, el.CoinTypeID, el.AppGoodID),
    Last30DaysUSDInComing: currency.currency(el.CoinTypeID) * profit.intervalGoodIncoming(undefined, logined.loginedUserID, IntervalKey.LastMonth, el.CoinTypeID, el.AppGoodID),
    GoodSaleEndAt: _good?.SaleEndAt,
    TotalEstimatedDailyReward: Number(el.Units) * parseFloat(good.good(undefined, el.AppGoodID)?.LastUnitRewardAmount as string),
    MiningStartDate: _good?.ServiceStartAt > Math.ceil(Date.now() / 1000) ? getTBD.value(el.AppGoodID) : utils.formatTime(_good?.ServiceStartAt, 'YYYY-MM-DD'),
    DaysMined: daysMined > durationDays ? durationDays : daysMined,
    DaysRemaining: daysRemaining
  } as MyGoodProfit
}).sort((a, b) => {
  if (a.CoinUnit.localeCompare(b.CoinUnit, 'zh-CN')) {
    return a.CoinUnit.localeCompare(b.CoinUnit, 'zh-CN')
  } else {
    return a.GoodSaleEndAt > b.GoodSaleEndAt ? -1 : 1
  }
}))
</script>
