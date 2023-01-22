<template>
  <div>
    <TodoApp :task="task"/>
  </div>
</template>

<script>
import TodoApp from './components/TodoApp.vue';

export default {
  name: 'App',
  props: {
    task: {
    
    }
  },
  components: {
    TodoApp
  }
}
</script>

<style>

</style>