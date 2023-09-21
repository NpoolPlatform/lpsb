<template>
  <ul class='language-picker'>
    <div v-if='langs.length > 1'>
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
import { applang, _locale, g11nbase } from 'src/npoolstore'
import { computed } from 'vue'

const lang = applang.useAppLangStore()
const langs = computed(() => lang.langs(undefined))

const locale = _locale.useLocaleStore()
const curLang = computed(() => locale.lang())

const onLangClick = (language: g11nbase.AppLang) => {
  locale.setLang(language)
}
</script>
