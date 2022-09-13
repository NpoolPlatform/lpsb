<template>
  <div class='row'>
    <div class='column justify-center'>
      <img class='table-coin-icon' :src='logo'>
    </div>
    <div class='column justify-center'>
      {{ name }}
    </div>
  </div>
</template>

<script setup lang='ts'>
import { defineProps, toRef } from 'vue'

interface Props {
  logo: string;
  name: string;
}

const props = defineProps<Props>()
const name = toRef(props, 'name')
const logo = toRef(props, 'logo')

</script>
<style lang='sass' scoped>
  .table-coin-icon
    width: 24px
    height: 24px
    margin-right: 10px
  </style>
