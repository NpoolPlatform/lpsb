<script setup lang='ts'>
import { computed, onMounted } from 'vue'
import { app, appgood, notify, appcoin } from 'src/npoolstore'
import { getCoins } from 'src/api/chain'

const good = appgood.useAppGoodStore()
const goods = computed(() => good.goods())

const application = app.useApplicationStore()

const coin = appcoin.useAppCoinStore()

onMounted(() => {
  if (!goods.value?.length) {
    getAppGoods(0, 100)
  }
  getApplication()
  if (!coin.coins(undefined)?.length) {
    getCoins(0, 100, () => {
      // TODO
    })
  }
})

const getAppGoods = (offset: number, limit: number) => {
  good.getAppGoods({
    Offset: offset,
    Limit: limit,
    Message: {
      Error: {
        Title: 'MSG_GET_APP_GOODS',
        Message: 'MSG_GET_APP_GOODS_FAIL',
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, (error: boolean, g?: Array<appgood.Good>) => {
    if (error || !g?.length) {
      return
    }
    getAppGoods(offset + limit, limit)
  })
}

const getApplication = () => {
  application.getApp({
    Message: {
      Error: {
        Title: 'MSG_GET_APP',
        Message: 'MSG_GET_APP_FAIL',
        Popup: true,
        Type: notify.NotifyType.Error
      }
    }
  }, () => {
    // TODO
  })
}

</script>
