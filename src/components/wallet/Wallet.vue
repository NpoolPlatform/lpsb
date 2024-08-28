<template>
  <div class='content'>
    <Balance />
    <div class='hr' />
    <Assets />
    <div class='hr' />
    <Transactions />
    <div class='hr' />
    <WithdrawRecords />
    <div class='hr' />
    <WithdrawAddresses />
    <div class='hr' />
    <TransferAccounts />
    <div class='hr' />
  </div>
  <q-ajax-bar
    ref='progress'
    position='top'
    color='blue-3'
    size='6px'
  />
</template>

<script setup lang="ts">
import {
  appcoin,
  ledger,
  transferaccount,
  coincurrency,
  fiatcurrency,
  notify,
  fiat,
  user,
  sdk,
  utils
} from 'src/npoolstore'
import { QAjaxBar } from 'quasar'
import { getCoins, getCurrencies } from 'src/api/chain'
import { defineAsyncComponent, onMounted } from 'vue'
import { useI18n } from 'vue-i18n'

// eslint-disable-next-line @typescript-eslint/unbound-method
const { t } = useI18n({ useScope: 'global' })

const Balance = defineAsyncComponent(() => import('src/components/wallet/Balance.vue'))
const Assets = defineAsyncComponent(() => import('src/components/wallet/Assets.vue'))
const Transactions = defineAsyncComponent(() => import('src/components/wallet/Transactions.vue'))
const WithdrawAddresses = defineAsyncComponent(() => import('src/components/wallet/WithdrawAddresses.vue'))
const WithdrawRecords = defineAsyncComponent(() => import('src/components/wallet/WithdrawRecords.vue'))
const TransferAccounts = defineAsyncComponent(() => import('src/components/wallet/TransferAccounts.vue'))

const general = ledger.useLedgerStore()
const coin = appcoin.useAppCoinStore()
const transfer = transferaccount.useTransferAccountStore()
const _coincurrency = coincurrency.useCurrencyStore()
const _fiatcurrency = fiatcurrency.useFiatCurrencyStore()
const logined = user.useLocalUserStore()

onMounted(() => {
  if (!general.ledgers().length) {
    getGenerals(0, 100)
  }
  if (!sdk.ledgerProfit.coinProfits(utils.IntervalKey.LastDay).length) {
    sdk.ledgerProfit.getCoinProfits(
      utils.IntervalKey.LastDay,
      utils.intervalStartAt(utils.IntervalKey.LastDay),
      Math.ceil(new Date().getTime() / 1000),
      0,
      0
    )
  }
  if (!coin.coins(undefined).length) {
    getCoins(0, 100)
  }
  if (!sdk.userWithdrawAccount.userWithdrawAccounts(logined.loginedUserID).length) {
    sdk.userWithdrawAccount.getMyUserWithdrawAccounts(0, 0)
  }
  if (!transfer.transferAccounts(undefined, logined.loginedUserID).length) {
    getTransfers(0, 100)
  }
  if (!_coincurrency.currencies().length) {
    getCurrencies(0, 500)
  }
  if (!_fiatcurrency.currencies().length) {
    getFiatCurrency()
  }
})

const getGenerals = (offset:number, limit: number) => {
  general.getLedgers({
    Offset: offset,
    Limit: limit,
    Message: {
      Error: {
        Title: t('MSG_GET_GENERAL_FAIL'),
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, (error: boolean, rows?: Array<ledger.Ledger>) => {
    if (error || !rows?.length) {
      return
    }
    getGenerals(limit + offset, limit)
  })
}

const getTransfers = (offset: number, limit: number) => {
  transfer.getTransfers({
    Offset: offset,
    Limit: limit,
    Message: {
      Error: {
        Title: t('MSG_GET_TRANSFER_ACCOUNTS_FAIL'),
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, (error: boolean, transfers?: Array<transferaccount.TransferAccount>) => {
    if (error || !transfers?.length) {
      return
    }
    getTransfers(limit + offset, limit)
  })
}

const getFiatCurrency = () => {
  _fiatcurrency.getFiatCurrency({
    FiatName: fiat.FiatType.JPY
  }, () => {
    // TODO
  })
}
</script>
