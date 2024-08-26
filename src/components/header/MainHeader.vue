<template>
  <header class='desktop1'>
    <img :src='logoText' class='attachment-large size-large logo  cursor-pointer' @click='onLogoClick'>
    <div class='nav'>
      <ul>
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/product/aleo" })'>{{ $t('MSG_HOME') }}</a></li>
        <!-- <li><a class='nav-link' href='javascript:void(0)'>{{ $t('MSG_BLOG') }}</a></li> -->
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/faqs" })'>{{ $t('MSG_SUPPORT_AND_FAQ') }}</a></li>
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/contact" })'>{{ $t('MSG_CONTACT') }}</a></li>
        <LangSwitcher />
        <SignHelper v-if='!localUser.logined' />
        <q-btn
          v-else
          size='1.1rem'
          flat dense round rounded
          :icon='"img:" + userAvatar'
          class='user-icon'
        >
          <q-menu
            self='top right'
            anchor='bottom right'
            transition-show='jump-down'
            transition-hide='jump-up'
          >
            <q-list class='dropdown'>
              <ExpandList
                :menu='menu'
                :show-icon='true'
                :show-icon-right='true'
                :show-label='false'
                :handle-router='false'
                :margin='true'
                @switch-menu='onSwitchMenu'
              />
            </q-list>
          </q-menu>
        </q-btn>
      </ul>
    </div>
  </header>

  <header class='mobile'>
    <img :src='logo' class='attachment-large size-large logo'>

    <div class='header-inner'>
      <LangSwitcher />
      <SignHelper v-if='!localUser.logined' />
      <q-btn
        v-else
        size='1.1rem'
        flat dense round rounded
        :icon='"img:" + userAvatar'
        class='user-icon'
      >
        <q-menu
          self='top right'
          anchor='bottom right'
          transition-show='jump-down'
          transition-hide='jump-up'
        >
          <q-list class='dropdown'>
            <ExpandList
              :menu='menu'
              :show-icon='true'
              :show-icon-right='true'
              :show-label='false'
              :handle-router='false'
              :margin='true'
              @switch-menu='onSwitchMenu'
            />
          </q-list>
        </q-menu>
      </q-btn>
    </div>
    <div class='hr' />
    <div class='nav'>
      <ul>
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/product/aleo" })'>{{ $t('MSG_HOME') }}</a></li>
        <!-- <li><a class='nav-link' href='javascript:void(0)'>{{ $t('MSG_BLOG') }}</a></li> -->
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/faqs" })'>{{ $t('MSG_SUPPORT_AND_FAQ') }}</a></li>
        <li><a class='nav-link' target='_blank' @click='router.push({ path: "/contact" })'>{{ $t('MSG_CONTACT') }}</a></li>
      </ul>
    </div>
  </header>
</template>
<script setup lang='ts'>
import { defineAsyncComponent, computed, watch, onMounted } from 'vue'
import { HeaderAvatarMenu, MenuItem } from 'src/menus/menus'
import { useRouter } from 'vue-router'
import { notify, user, localapp } from 'src/npoolstore'

import logoText from '../../assets/logo-text.png'
import logo from '../../assets/logo-icon.png'
import userAvatar from '../../assets/user-circle.svg'

const LangSwitcher = defineAsyncComponent(() => import('src/components/lang/LangSwitcher.vue'))
const SignHelper = defineAsyncComponent(() => import('src/components/header/SignHelper.vue'))
const ExpandList = defineAsyncComponent(() => import('src/components/list/ExpandList.vue'))

const _user = user.useUserStore()
const localUser = user.useLocalUserStore()

const router = useRouter()

const onSwitchMenu = (item: MenuItem) => {
  if (item.label === 'MSG_LOGOUT') {
    _user.logout({
      Token: localUser.User.LoginToken,
      Message: {
        Error: {
          Title: 'MSG_LOGOUT_FAIL',
          Popup: true,
          Type: notify.NotifyType.Error
        }
      }
    }, () => {
      // TODO
    })
    void router.push({ path: '/' })
    return
  }

  void router.push({
    path: item.target,
    query: {
      menuId: item.menuId
    }
  })
}

const menu = computed(() => {
  const myMenu = HeaderAvatarMenu()
  myMenu.children = myMenu.children.filter((m) => m.label !== 'MSG_REFERRAL' || localUser.isKol)
  return myMenu
})

const onLogoClick = () => {
  void router.push({ path: '/product/aleo' })
}

const logined = computed(() => localUser.logined)

watch(logined, () => {
  if (!logined.value) {
    return
  }
  setTimeout(() => {
    initialize()
  }, 2000)
})

const initialize = () => {
  if (localUser.User?.InvitationCode?.length) {
    // TODO
  }
}

onMounted(() => {
  if (logined.value) {
    setTimeout(() => {
      initialize()
    }, 2000)
  }
})

const app = localapp.useLocalApplicationStore()
const maintaining = computed(() => app.maintaining())

watch(maintaining, () => {
  if (maintaining.value) {
    void router.push({ path: '/maintenance' })
  }
})

onMounted(() => {
  if (maintaining.value) {
    void router.push({ path: '/maintenance' })
  }
})

</script>

<style lang='sass' scoped>
.dropdown
  min-width: 200px
</style>
