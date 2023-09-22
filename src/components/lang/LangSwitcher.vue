<template>
  <ul class='language-picker'>
    <div>
      <li
        v-for='language in langs'
        :class='[ curLang === language.Lang ? "selected" : "" ]'
        :key='language.ID'
        @click='onLangClick(language)'
      >
        <a class='language'>
          {{ language.Short?.length > 0 ? language.Short : language.Lang }}
        </a>
      </li>
    </div>
  </ul>
</template>

<script setup lang='ts'>
import { Cookies } from 'quasar'
import { applang, _locale, g11nbase, user } from 'src/npoolstore'
import { computed, watch } from 'vue'

const lang = applang.useAppLangStore()
const langs = computed(() => lang.langs(undefined))

const locale = _locale.useLocaleStore()
const curLang = computed(() => locale.lang())

const logined = user.useLocalUserStore()
const _user = user.useUserStore()

const onLangClick = (language: g11nbase.AppLang) => {
  locale.setLang(language)
  if (logined.logined) {
    _user.updateUser({
      SelectedLangID: language.LangID,
      Message: {}
    }, () => {
      // TODO
    })
  }
}

watch([() => logined?.selectedLangID, () => langs.value?.length], () => {
  if (logined?.logined && logined?.selectedLangID?.length > 0) {
    const _lang = lang.langs(undefined).find((el) => el.LangID === logined?.selectedLangID)
    if (_lang) {
      locale.setLang(_lang)
      return
    }
  }
  const langID = Cookies.get('X-Lang-ID')
  if (langID && langID?.length > 0) {
    const _lang = lang.langs(undefined).find((el) => el.LangID === langID)
    if (_lang) {
      locale.setLang(_lang)
    }
  }
})

</script>
