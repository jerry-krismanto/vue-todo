<!-- TODO:
1. add update method to update task status when clicked (DONE!!!)
2. add a filter to show only completed tasks -->

<script setup>
import TodoList from './components/TodoList.vue'
import { ref, onMounted, computed } from 'vue'

const newTodo = ref('')
const todos = ref([])
const hideCompleted = ref(false)

const fetchTodos = async () => {
  try {
    const response = await fetch('http://localhost:3000/todos')
    todos.value = await response.json()
  } catch (err) {
    console.log('error fetching data, ', err)
  }
}

const postTodos = async () => {
  try {
    const response = await fetch('http://localhost:3000/todos', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ text: newTodo.value, completed: false })
    })
    const data = await response.json()
    todos.value.push(data)
    newTodo.value = ''
  } catch (err) {
    console.log('error posting data, ', err)
  }
}

const deleteTodos = async (id) => {
  try {
    const response = await fetch(`http://localhost:3000/todos/${id}`, {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ text: newTodo.value, completed: false })
    })
    if (response.ok) {
      todos.value = todos.value.filter((todo) => todo.id !== id)
    } else {
      console.log('error deleting todo')
    }
  } catch (err) {
    console.log('error deleting data, ', err)
  }
}

const updateTodosStatusComplete = async (id) => {
  try {
    const response = await fetch(`http://localhost:3000/todos/${id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ completed: true })
    })
    const data = await response.json()
    console.log(data)
    if (response.ok) {
      console.log('todo updated')
    } else {
      console.log('error updating todo')
    }
  } catch (err) {
    console.log('error updating todo', err)
  }
}

const updateTodosStatusPending = async (id) => {
  try {
    const response = await fetch(`http://localhost:3000/todos/${id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ completed: false })
    })
    const data = await response.json()
    console.log(data)
    if (response.ok) {
      console.log('todo updated')
    } else {
      console.log('error updating todo')
    }
  } catch (err) {
    console.log('error updating todo', err)
  }
}

const filterCompleted = computed(() => {
  return hideCompleted.value ? todos.value.filter((todo) => todo.completed === false) : todos.value
})

onMounted(() => {
  fetchTodos()
})

</script>

<template>
  <div class="flexbox">
    <h1>To do app</h1>
    <input
      class="todo-input"
      v-model="newTodo"
      @keyup.enter="postTodos"
      placeholder="Add a new Todo"
    />
    <div>
      <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
    </div>
    <TodoList
      :todos="todos"
      :filterCompleted="filterCompleted"
      @delete-todo="deleteTodos"
      @update-status-complete="updateTodosStatusComplete"
      @update-status-pending="updateTodosStatusPending"
    />
  </div>
</template>

<style scoped>
.flexbox {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 70vw;
  margin: 0 auto;
}

h1 {
  margin: 2rem;
  font-weight: 500;
}

.todo-input {
  width: 100%;
  padding: 1rem;
  font-size: 1.2rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 1rem;
}

@media screen and (max-width: 500px) {
  .flexbox {
    width: 70vw;
  }
}
</style>
