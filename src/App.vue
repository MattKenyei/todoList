<script lang="ts">
import { defineComponent } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFilters from './components/AppFilters.vue'
import AppToDoList from './components/AppToDoList.vue'
import AppForm from './components/AppForm.vue'
import AppFooter, {  type Stats } from './components/AppFooter.vue'
import type { ToDo } from '@/types/todo'
import type { Filter } from './types/filter'

interface State {
  todos: ToDo[]
  activeFilter: Filter
}
export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppToDoList,
    AppForm,
    AppFooter
  },
  methods: {
    addGoal(goal: ToDo) {
      this.todos.push(goal)
      localStorage.setItem('tasks', JSON.stringify(this.todos))
    },
    toggleGoal(id: number) {
      const Target = this.todos.find((todo: ToDo) => todo.id === id)
      if (Target) Target.completed = !Target.completed
      localStorage.setItem('tasks', JSON.stringify(this.todos))
    },
    RemoveGoal(id: number) {
      this.todos = this.todos.filter((todo: ToDo) => todo.id !== id)
      localStorage.setItem('tasks', JSON.stringify(this.todos))
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    }
  },
  computed: {
    filteredGoals(): ToDo[] {
      switch (this.activeFilter) {
        
        case 'Active':
          return this.activeGoals
        case 'Done': return this.doneGoals
        case 'All':
        default: return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.activeGoals.length,
        done: this.doneGoals.length
      }
    },
    activeGoals(): ToDo[] {
      return this.todos.filter((todo) => !todo.completed)
    },
    doneGoals(): ToDo[] {
      return this.todos.filter((todo) => todo.completed)
    }
  },
  data(): State {
    return {
      todos: [],
      // { id: 0, text: 'Learn the basics of Vue', completed: true },
      //   { id: 1, text: 'Learn the basics of Typescript', completed: false },
      //   { id: 2, text: 'Push it', completed: false }
      activeFilter: 'All'
    }
  },
  mounted() {
    const storedTasks = localStorage.getItem('tasks')
    if (storedTasks) this.todos = JSON.parse(storedTasks)
  }
})
</script>

<template>
  <AppHeader />

  <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />

  <main class="app-main">
    <AppToDoList :todos="filteredGoals" @toggle="toggleGoal" @remove="RemoveGoal" />

    <AppForm @add-goal="addGoal" />
  </main>

  <AppFooter :stat="stats"/>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

}
</style>
