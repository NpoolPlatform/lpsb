<template>
  <h2>{{ $t("MSG_EARNINGS") }}</h2>
  <div class='earnings-summary'>
    <div class='earnings-figure'>
      <span class='amount'>{{ totalProfit }}</span>
      <span class='unit'>{{ constant.PriceCoinName }}</span>
      <div class='hr' />
      <h4 class='description'>
        {{ $t("MSG_TOTAL_EARNINGS") }}
      </h4>
    </div>
    <div class='earnings-figure'>
      <span class='amount'>{{ last24HoursEarning }}</span>
      <span class='unit'>{{ constant.PriceCoinName }}</span>
      <div class='hr' />
      <h4 class='description'>
        {{ $t("MSG_YESTERDAY_EARNING") }}
      </h4>
    </div>
  </div>
</template>

<script setup lang="ts">
import { IntervalKey } from 'src/const/const'
import { computed } from 'vue'
import { constant, coincurrency, ledgerprofit, user } from 'src/npoolstore'

const currency = coincurrency.useCurrencyStore()
const logined = user.useLocalUserStore()

const profit = ledgerprofit.useProfitStore()
const profits = computed(() => profit.profits(undefined, logined.loginedUserID))
const intervalProfits = computed(() => profit.intervalProfits(undefined, logined.loginedUserID, undefined, IntervalKey.LastDay))

const totalProfit = computed(() => {
  let total = 0
  profits.value.forEach((el) => {
    total += Number(el.Incoming) * currency.currency(el.CoinTypeID)
  })
  return total.toFixed(4)
})

const last24HoursEarning = computed(() => {
  let total = 0
  intervalProfits.value.forEach((el) => {
    total += Number(el.Incoming) * currency.currency(el.CoinTypeID)
  })
  return total.toFixed(4)
})

</script>
