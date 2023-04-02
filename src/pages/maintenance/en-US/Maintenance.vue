<template>
  <div class='maintenance'>
    <div>
      <img :src='lightLogo' @click='onLogoClick'>
      <h2>Thank you for visiting our website.</h2>
      <p>We are currently undergoing a system update and our service is temporarily unavailable. We apologize for any inconvenience this may cause and appreciate your understanding.</p>
    </div>
  </div>
</template>
<script setup lang='ts'>
import { NotifyType, useFrontendAppStore } from 'npool-cli-v4'
import lightLogo from 'src/assets/logo-text.png'
import { AppID } from 'src/const/const'
import { useLocalAppStore } from 'src/localstore/app'
import { onMounted } from 'vue'
import { useI18n } from 'vue-i18n'
import { useRouter } from 'vue-router'
// eslint-disable-next-line @typescript-eslint/unbound-method
const { t } = useI18n({ useScope: 'global' })

const router = useRouter()
const onLogoClick = () => {
  void router.push({ path: '/' })
}

const app = useLocalAppStore()
const application = useFrontendAppStore()
const getApplication = () => {
  application.getApp({
    AppID: AppID,
    Message: {
      Error: {
        Title: t('MSG_GET_APP_FAIL'),
        Popup: true,
        Type: NotifyType.Error
      }
    }
  }, () => {
    app.setApp(application.App)
  })
}

onMounted(() => {
  getApplication()
})

</script>
