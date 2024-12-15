<template>
  <footer class="app-footer">{{ stat.active }} more to do, {{ stat.done }} done</footer>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue'

export interface Stats {
  active: number
  done: number
}

export default defineComponent({
  props: {
    stat: {
      type: Object as PropType<Stats>,
      required: true
    }
  }
})
</script>
