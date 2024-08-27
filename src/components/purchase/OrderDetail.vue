<template>
  <div :class='[ showStatus || showWarning ? "blur" : "" ]'>
    <PurchasePage :app-good-id='_order?.AppGoodID as string'>
      <div class='info'>
        <h3 class='form-title'>
          {{ sdk.appCoin.displayName(sdk.appPowerRental.mainCoinTypeID(_order?.AppGoodID as string) as string, 0) }} | <strong>{{ $t('MSG_ORDER_ID') }}: {{ orderId }}</strong>
        </h3>
        <div class='info-flex'>
          <div class='three-section'>
            <h4>{{ $t('MSG_PURCHASE_AMOUNT') }}:</h4>
            <span class='number'>{{ _order?.Units }}</span>
            <span class='unit'>{{ $t(sdk.powerRentalOrder.orderGoodQuantityUnit(_order?.OrderID as string)) }}</span>
          </div>
          <div class='three-section'>
            <h4>{{ $t('MSG_AMOUNT_DUE') }}:</h4>
            <span class='number'>{{ sdk.powerRentalOrder.powerRentalOrder(orderId)?.PaymentBalances?.[0]?.Amount || _order?.GoodValueUSD }}</span>
            <span class='unit'>{{ sdk.powerRentalOrder.powerRentalOrder(orderId)?.PaymentBalances?.[0]?.CoinUnit || constant.PriceCoinName }}</span>
            <img class='copy-button' :src='copyIcon' @click='onCopyAmountClick'>
            <div class='tooltip'>
              <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
              <p class='tooltip-text'>
                {{ $t('MSG_PAYMENT_AMOUNT_TIP') }}
              </p>
            </div>
          </div>
          <div class='three-section'>
            <h4>{{ $t('MSG_TIME_REMAIN') }}:</h4>
            <span class='number'>{{ $t(remainSeconds) }}</span>
            <div class='tooltip'>
              <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
              <p class='tooltip-text'>
                {{ $t('MSG_PAYMENT_TIMEOUT_TIP') }}
              </p>
            </div>
          </div>
          <div class='full-section'>
            <h4>{{ $t('MSG_PAYMENT_ADDRESS') }}:</h4>
            <div v-if='showBUSDTip' class='warning warning-pink'>
              <img :src='warning'>
              <span v-html='$t("MSG_BUSD_PAYMENT_TIP")' />
            </div>
            <div v-else class='warning warning-pink'>
              <img :src='warning'>
              <span v-html='$t("MSG_COIN_PAYMENT_TIP", { COIN_NAME: paymentCoinName })' />
            </div>
            <!-- <span class='wallet-type'>{{ coinName }} &nbsp; </span> -->
            <!-- <span class='number'>  {{ order?.PaymentAddress }}</span> -->
            <!-- <img class='copy-button' :src='copyIcon' @click='onCopyAddressClick'> -->
            <!-- <div class='tooltip'>
              <img class='more-info' :src='question'><span>{{ $t('MSG_LEARN_MORE') }}</span>
              <p class='tooltip-text'>
                {{ $t('MSG_PAYMENT_ADDRESS_TIP') }}
              </p>
            </div> -->
          </div>
        </div>
        <div class='hr' />
        <h4>{{ $t('MSG_IMPORTANT_INFORMATION') }}</h4>
        <p v-html='$t("MSG_PAYMENT_NOTE", { COIN_NAME: paymentCoinName })' />
      </div>
      <div class='order-form'>
        <!-- <h3 class='form-title'>
          {{ $t('MSG_SCAN_QR_CODE_TO_PAY') }}
        </h3>
        <div class='qr-code-container'>
          <div class='qr-code-container' ref='qrCodeContainer'>
            <h5>{{ coinName }}  {{ $t('MSG_ADDRESS') }}</h5>
            <qrcode-vue
              :value='order?.PaymentAddress'
              :size='qrCodeContainer?.clientWidth as number - 1'
              :margin='3'
              class='qr-code'
            />
          </div>
        </div> -->
        <div class='hr' />
        <button @click='onPaymentProceed'>
          {{ $t('MSG_PROCEED_TO_DASHBOARD') }}
        </button>
        <button @click='onPaymentCanceled' class='alt'>
          {{ $t('MSG_CANCEL_ORDER') }}
        </button>
        <div class='warning'>
          <img :src='warning'>
          <span>{{ $t('MSG_PAYMENT_WARNING') }}</span>
        </div>
      </div>
    </PurchasePage>
  </div>
  <q-dialog
    v-model='showStatus'
    seamless
    maximized
  >
    <div class='product-container content-glass popup-container'>
      <div class='popup'>
        <PaymentState
          :order-id='query.orderId'
          :title='popupTitle'
          :tip-message='tipMessage'
          :state='orderStatus'
          :show-type='showType'
          :remain-time='remainTime'
          @proceed='onPaymentProceed'
        />
      </div>
    </div>
  </q-dialog>
  <q-dialog
    v-model='showWarning'
    seamless
    maximized
  >
    <div class='product-container content-glass popup-container'>
      <div class='popup'>
        <div class='form-container content-glass'>
          <div class='confirmation'>
            <h3>{{ $t('MSG_PAYMENT_WARNING_TITLE') }}</h3>
            <p v-html='$t("MSG_PAYMENT_WARNING_CONTENT_1")' />
            <div class='warning normal-tip'>
              <img :src='warning'>
              <span>{{ $t('MSG_PAYMENT_WARNING_CONTENT_2') }}</span>
            </div>
            <p v-html='$t("MSG_PAYMENT_WARNING_CONTENT_3")' />
            <button @click='showWarning = false'>
              {{ $t('MSG_I_UNDERSTAND') }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </q-dialog>
  <q-dialog
    v-model='showCancelling'
    seamless
    maximized
  >
    <div class='product-container content-glass popup-container'>
      <div class='popup'>
        <div class='form-container content-glass'>
          <div class='confirmation'>
            <h3>{{ $t('MSG_ORDER_CANCELLATION_TITLE') }}</h3>
            <p v-html='$t("MSG_ORDER_CANCELLATION_CONTENT_1")' />
            <div class='warning red-warning'>
              <img :src='warning'>
              <span v-html='$t("MSG_ORDER_CANCELLATION_CONTENT_2")' />
            </div>
            <button @click='showCancelling = false' class='alt'>
              {{ $t('MSG_GO_BACK') }}
            </button>
            <button @click='onCancelOrderClick'>
              {{ $t('MSG_CANCEL_ORDER') }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </q-dialog>
</template>

<script setup lang='ts'>
import { defineAsyncComponent, computed, ref, onMounted, onUnmounted, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import copy from 'copy-to-clipboard'
import { order, utils, notify, constant, sdk, powerrentalorder } from 'src/npoolstore'
import copyIcon from 'src/assets/icon-copy.svg'
import question from 'src/assets/question.svg'
import warning from 'src/assets/warning.svg'

const PurchasePage = defineAsyncComponent(() => import('src/components/purchase/PurchasePage.vue'))
const PaymentState = defineAsyncComponent(() => import('src/components/purchase/PaymentState.vue'))

interface Query {
  orderId: string
}

const route = useRoute()
const query = computed(() => route.query as unknown as Query)
const orderId = computed(() => query.value.orderId)

const odr = order.useOrderStore()
const _order = computed(() => sdk.powerRentalOrder.powerRentalOrder(orderId.value))

const notification = notify.useNotificationStore()

const remainSeconds = ref(odr.orderState(_order.value?.ID as number))
const ticker = ref(-1)
const counter = ref(0)

const showStatus = ref(false)
const popupTitle = ref('')
const tipMessage = ref('')
const orderStatus = ref('')
const showType = ref('')
const remainTime = ref(order.RemainMax)
const showWarning = ref(true)
const showCancelling = ref(false)

const paymentCoinName = computed(() => sdk.appCoin.displayName(_order.value?.PaymentBalances?.[0]?.CoinTypeID as string, 0))
const showBUSDTip = computed(() => paymentCoinName.value?.toLowerCase()?.includes('binanceusd'))

const remainTicker = ref(-1)

watch(counter, () => {
  if (counter.value % 30 === 0) {
    sdk.powerRentalOrder.getPowerRentalOrder(orderId.value)
  }
})

const launchTicker = () => {
  ticker.value = window.setInterval(() => {
    remainSeconds.value = odr.orderState(_order.value?.ID as number)

    if (odr.orderPaid(_order.value?.ID as number)) {
      showStatus.value = true
      popupTitle.value = 'MSG_ORDER_COMPLETE'
      tipMessage.value = 'MSG_REVIEW_ORDER'
      orderStatus.value = 'MSG_COMPLETE'
      showType.value = 'date'
      if (remainTicker.value >= 0) {
        clearInterval(remainTicker.value)
        remainTicker.value = -1
      }
      return
    }

    let start = _order.value ? _order.value?.CreatedAt : 0
    start += order.OrderTimeoutSeconds
    remainTime.value = utils.remain(start)
    if (remainTime.value === utils.RemainZero) {
      showStatus.value = true
      popupTitle.value = 'MSG_ORDER_TIMEOUT'
      tipMessage.value = 'MSG_ORDER_TIMEOUT'
      orderStatus.value = 'MSG_TIMEOUT'
      if (remainTicker.value >= 0) {
        clearInterval(remainTicker.value)
        remainTicker.value = -1
      }
    }
    counter.value++
  }, 1000)
}

onMounted(() => {
  sdk.powerRentalOrder.getPowerRentalOrder(orderId.value, (error: boolean) => {
    if (error || !sdk.powerRentalOrder.validate(orderId.value)) {
      return
    }
    launchTicker()
  })
})

onUnmounted(() => {
  if (ticker.value >= 0) {
    window.clearInterval(ticker.value)
  }
  odr.$reset()
})

const router = useRouter()

const onPaymentProceed = () => {
  void router.push({
    path: '/dashboard'
  })
}

const onPaymentCanceled = () => {
  showCancelling.value = true
}

const onCancelOrderClick = () => {
  showCancelling.value = false
  sdk.powerRentalOrder.updatePowerRentalOrder(_order.value as powerrentalorder.PowerRentalOrder, undefined, true, (error: boolean) => {
    if (error) return
    void router.push({
      path: '/dashboard'
    })
  })
}

const onCopyAmountClick = () => {
  copy(_order.value?.PaymentBalances?.[0]?.Amount as string)
  notification.Notifications.push({
    Title: 'MSG_AMOUNT_COPIED',
    Message: 'MSG_COPY_AMOUNT_SUCCESS',
    Popup: true,
    Type: notify.NotifyType.Success
  })
}

// const onCopyAddressClick = () => {
//   copy(_order.value?.PaymentAddress as string)
//   notification.Notifications.push({
//     Title: 'MSG_ADDRESS_COPIED',
//     Message: 'MSG_COPY_ADDRESS_SUCCESS',
//     Popup: true,
//     Type: notify.NotifyType.Success
//   })
// }

</script>

<style lang='sass' scoped>
.qr-code-container
  margin: 24px 0
  text-align: center
  width: 100%

.qr-code
  border-radius: 0 0 12px 12px
  width: 100%

.qr-code-container h5
  background: #fc4468
  border-radius: 12px 12px 0 0
  font-size: 20px
  font-weight: 400
  margin: 0
  padding: 4px
  text-transform: uppercase

.product-container
  background: transparent

.normal-tip
  padding: 12px
  margin: 24px 0

::v-deep p:not([class]) strong
  background: linear-gradient(to left, #ff964a 0, #ffcc4a 50%)
  background-clip: border-box
  font-weight: 500
  -webkit-background-clip: text
  -webkit-box-decoration-break: clone
  -webkit-text-fill-color: transparent
</style>
