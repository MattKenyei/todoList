<template>
  <ul class="todo-list">
    <AppToDoItem v-for="todo in todos" :key="todo.id" :todo="todo" @toggle="toggleGoal" @remove="RemoveGoal" />
  </ul>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue'
import AppToDoItem from './AppToDoItem.vue'
import type { ToDo } from '@/types/todo';


export default defineComponent({
  components: {
    AppToDoItem
  },
  props: {
    todos: {
      type: Array as PropType<ToDo[]>
    }
  },
  methods: {
    toggleGoal(id: number) {
      this.$emit('toggle', id)
    },
    RemoveGoal(id: number){
      this.$emit('remove', id)
    }
  },
  emits: {
    toggle: (id: number) => Number.isInteger(id),
    remove: (id: number) => Number.isInteger(id),
  }
})
</script>
