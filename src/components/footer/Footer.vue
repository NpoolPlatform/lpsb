<template>
  <footer>
    <div class='content' :class='[ special ? "special" : "" ]'>
      <div class='columns-4'>
        <div class='column-4'>
          <img class='logo' :src='logoText'>
          <h1>
            <!-- <span v-html='$t("MSG_SHOOT_FOT_THE_STARS")' /> -->
          </h1>
          <h4 class='tagline'>
            <!-- <span v-html='$t("MSG_YOUR_GLOBAL_CONSTELLATION")' /> -->
          </h4>
        </div>
        <div class='column-4' v-if='langs.length > 1'>
          <h4>{{ $t('MSG_LANGUAGE') }}</h4>
          <LangSwitcher />
        </div>
        <div v-else class='column-4'>
          <h4 />
        </div>
        <div class='column-4'>
          <h4>{{ $t('MSG_LINKS') }}</h4>
          <ul>
            <li><a class='footer-link' href='#/product/aleo'>{{ $t('MSG_HOME') }}</a></li>
            <li><a class='footer-link' href='#/product/aleo'>{{ $t('MSG_PRODUCTS') }}</a></li>
            <!-- <li><a class='footer-link' href='javascript:void(0)'>{{ $t('MSG_BLOG') }}</a></li> -->
          </ul>
        </div>
        <div class='column-4'>
          <h4>{{ $t('MSG_HELP') }}</h4>
          <ul>
            <li><a class='footer-link' href='#/faqs'>{{ $t('MSG_FAQ') }}</a></li>
            <li><a class='footer-link' href='#/contact'>{{ $t('MSG_CONTACT') }}</a></li>
            <li><a class='footer-link' href='#/legal'>{{ $t('MSG_LEGAL') }}</a></li>
          </ul>
        </div>
      </div>
      <p class='copyright'>
        © 2022 LP Secret Base
      </p>
    </div>
  </footer>
</template>

<script setup lang='ts'>
import { computed, defineAsyncComponent } from 'vue'
import { _locale, applang } from 'src/npoolstore'
import logoText from '../../assets/logo-text.png'
const LangSwitcher = defineAsyncComponent(() => import('src/components/lang/LangSwitcher.vue'))
const locale = _locale.useLocaleStore()
const special = computed(() => locale.lang() === 'ja-JP')

const _applang = applang.useAppLangStore()
const langs = computed(() => _applang.langs(undefined))
</script>
