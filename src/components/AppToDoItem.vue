<template>
  <li class="todo-item" :class="{ ' todo-item--done': todo.completed }" @click="toggle">
    <div class="todo-item__status">
      <i class="bi bi-check2"></i>
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <button class="todo-item__remove-button" @click.stop="deleteGoal">
      <i class="bi bi-trash3"></i>
    </button>
  </li>
</template>

<script lang="ts">
import type { ToDo } from '@/types/todo'
import { defineComponent, type PropType } from 'vue'

export default defineComponent({
  props: {
    todo: {
      type: Object as PropType<ToDo>,
      required: true
    }
  },
  methods: {
    toggle() {
      this.$emit('toggle', this.todo.id)
    },
    deleteGoal() {
        this.$emit('remove', this.todo.id)
    }
  },
  emits: {
    toggle: (id: number) => Number.isInteger(id),
    remove: (id: number) => Number.isInteger(id),
  }
})
</script>
