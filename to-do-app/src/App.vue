<script setup>
import TodoList from './components/TodoList.vue';
import { ref } from 'vue';

const newTodo = ref('');
const todos = ref([]);

const addTodo = () => {
  if(newTodo.value === '') return;
  todos.value.push({text: newTodo.value, done: false});
  newTodo.value = '';
};  

</script>

<template>
  <div class="flexbox">
    <h1>To do app</h1>
    <input class="todo-input" v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new Todo">
    <TodoList :todos="todos" />
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

</style>
