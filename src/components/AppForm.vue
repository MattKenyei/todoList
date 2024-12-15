<template>
  <section class="add-todo">
    <form v-if="isVisible" class="add-todo__form" @submit.prevent="addGoal">
      <button class="close-button" type="button" @click="CloseForm">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <input v-model="goalText" class="input" />
      </div>
      <button class="button button--filled">Add task</button>
    </form>
    <button v-else class="add-todo__show-form-button" @click="OpenForm">
      <i class="bi bi-plus-lg"></i>
    </button>
  </section>
</template>

<script lang="ts">
import type { ToDo } from '@/types/todo'
import { defineComponent } from 'vue'

interface State {
  isVisible: boolean
  goalText: string
}

export default defineComponent({
  data(): State {
    return {
      isVisible: false,
      goalText: ''
    }
  },
  methods: {
    OpenForm() {
      this.isVisible = true
    },
    CloseForm() {
      this.isVisible = false
    },
    addGoal() {
      this.$emit('addGoal', {
        id: Date.now(),
        text: this.goalText,
        completed: false
      })
      this.goalText = ''
    }
  },
  emits: {
    addGoal: (goal: ToDo) => goal
  }
})
</script>
