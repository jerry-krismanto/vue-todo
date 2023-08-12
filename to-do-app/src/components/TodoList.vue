<template>
  <table>
    <thead>
      <tr>
        <th class="todo-name">Task</th>
        <th>Status</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :todo="todo"
        @delete-todo="deleteTodo"
      />
    </tbody>
  </table>
</template>

<script setup>
import TodoItem from './TodoItem.vue'
const props = defineProps({
  todos: Array
})
props //this line is only used to prevent error message, you can ignore it
const emit = defineEmits(['delete-todo'])
const deleteTodo = (id) => {
  emit('delete-todo', id)
}
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
  grid-template-columns: 5fr 1fr 1fr;
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

.todo-name {
  width: 400px;
}

@media screen and (max-width: 500px) {
  table {
    /* display: block; */
    width: 70vw;
  }

  thead {
    width: 70vw;
  }

  tbody {
    width: 70vw;
  }

  tr {
    grid-template-columns: auto auto auto;
  }

  th {
    width: 100px;
  }

  td {
    width: 100px;
  }

  .todo-name {
    width: 140px;
  }
}
</style>
