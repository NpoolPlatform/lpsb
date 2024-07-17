<template>
  <div class='content'>
    <!-- <MiningSummary /> -->
    <!-- <div class='hr' /> -->
    <MiningCards />
    <div class='hr' />
    <Orders />
    <div class='hr' />
  </div>
  <q-ajax-bar
    ref='progress'
    position='top'
    color='blue-3'
    size='6px'
  />
</template>

<script setup lang='ts'>
import { defineAsyncComponent, onMounted, computed } from 'vue'
import { useI18n } from 'vue-i18n'
import { QAjaxBar } from 'quasar'
import { appgood, notify, appcoin, coincurrency, ledgerstatement, user, sdk, utils } from 'src/npoolstore'
import { getCoins, getCurrencies } from 'src/api/chain'

// const MiningSummary = defineAsyncComponent(() => import('src/components/dashboard/MiningSummary.vue'))
const MiningCards = defineAsyncComponent(() => import('src/components/dashboard/MiningCards.vue'))
const Orders = defineAsyncComponent(() => import('src/components/dashboard/Orders.vue'))

// eslint-disable-next-line @typescript-eslint/unbound-method
const { t } = useI18n({ useScope: 'global' })

const coin = appcoin.useAppCoinStore()
const good = appgood.useAppGoodStore()
const currency = coincurrency.useCurrencyStore()
const logined = user.useLocalUserStore()

const detail = ledgerstatement.useStatementStore()
const miningRewards = computed(() => detail.miningRewards(undefined, logined.loginedUserID))
onMounted(() => {
  if (!sdk.ledgerProfit.coinProfits(utils.IntervalKey.All).length) {
    sdk.ledgerProfit.getCoinProfits(utils.IntervalKey.All, 0, Math.ceil(new Date().getTime() / 1000), 0, 0)
  }
  if (!sdk.ledgerProfit.coinProfits(utils.IntervalKey.LastDay).length) {
    sdk.ledgerProfit.getCoinProfits(
      utils.IntervalKey.All,
      utils.intervalStartAt(utils.IntervalKey.LastDay),
      Math.ceil(new Date().getTime() / 1000),
      0,
      0
    )
  }

  if (!sdk.ledgerProfit.goodProfits(utils.IntervalKey.All).length) {
    sdk.ledgerProfit.getGoodProfits(utils.IntervalKey.All, 0, Math.ceil(new Date().getTime() / 1000), 0, 0)
  }
  if (!sdk.ledgerProfit.goodProfits(utils.IntervalKey.LastDay).length) {
    sdk.ledgerProfit.getGoodProfits(
      utils.IntervalKey.All,
      utils.intervalStartAt(utils.IntervalKey.LastDay),
      Math.ceil(new Date().getTime() / 1000),
      0,
      0
    )
  }
  if (!sdk.ledgerProfit.goodProfits(utils.IntervalKey.LastMonth).length) {
    sdk.ledgerProfit.getGoodProfits(
      utils.IntervalKey.All,
      utils.intervalStartAt(utils.IntervalKey.LastMonth),
      Math.ceil(new Date().getTime() / 1000),
      0,
      0
    )
  }

  if (!sdk.powerRentalOrder.powerRentalOrders.value.length) {
    sdk.powerRentalOrder.getMyPowerRentalOrders(0, 0)
  }

  if (!good.goods(undefined).length) {
    getAppGoods(0, 100)
  }

  if (!coin.coins(undefined).length) {
    getCoins(0, 100)
  }
  if (!currency.currencies()) {
    getCurrencies(0, 100)
  }

  if (miningRewards.value?.length === 0) {
    getMiningRewards(0, 100)
  }
})

const getAppGoods = (offset: number, limit: number) => {
  good.getAppGoods({
    Offset: offset,
    Limit: limit,
    Message: {
      Error: {
        Title: 'MSG_GET_APP_GOODS_FAIL',
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, (error: boolean, rows?: Array<appgood.Good>) => {
    if (error || !rows?.length) {
      return
    }
    getAppGoods(offset + limit, limit)
  })
}

const getMiningRewards = (offset: number, limit: number) => {
  detail.getMiningRewards({
    Offset: offset,
    Limit: limit,
    EndAt: Math.ceil(Date.now() / 1000),
    Message: {
      Error: {
        Title: t('MSG_REWARD_FAIL'),
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, (error: boolean, rows?: Array<ledgerstatement.MiningReward>) => {
    if (error || !rows?.length) {
      return
    }
    getMiningRewards(limit + offset, limit)
  })
}

</script>

<style lang='sass' scoped>
.order
  max-width: 1040px
</style>
