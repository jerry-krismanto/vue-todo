<template>
  <!-- <ul style="list-style: number;">
        <TodoItem v-for="(todo, index) in todos" :key="index" :todo="todo" />
    </ul> -->
  <table>
    <thead>
      <tr>
        <th>Task</th>
        <th>Status</th>
      </tr>
    </thead>
    <tbody>
      <TodoItem v-for="(todo, index) in todos" :key="index" :todo="todo" />
    </tbody>
  </table>
</template>

<script setup>
import TodoItem from './TodoItem.vue'
const props = defineProps({
  todos: Array
})
props //this line is only used to prevent error message, you can ignore it
</script>

<style>
table {
  border-collapse: collapse;
  width: 100%;
  margin: 0 auto;
  text-align: center;
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
  justify-content: space-between;
}

tr {
  display: grid;
  grid-template-columns: auto auto;
  justify-content: space-around;
}

thead > tr {
  border-bottom: 1px solid #ccc;
}

th,
td {
  padding: 8px;
  text-align: center;
  width: 150px;
  margin-top: 1rem;
}
</style>
