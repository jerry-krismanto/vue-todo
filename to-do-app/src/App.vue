<script setup>
import TodoList from './components/TodoList.vue'
import { ref, onMounted } from 'vue'

const newTodo = ref('')
const todos = ref([])

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
    <TodoList :todos="todos" @delete-todo="deleteTodos" />
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
