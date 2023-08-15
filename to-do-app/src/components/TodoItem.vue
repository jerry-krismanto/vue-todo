<!-- eslint-disable vue/no-mutating-props -->
<template>
  <tr class="item-list">
    <td class="todo-name">
      <span class="todo-text">{{ todo.text }}</span>
    </td>
    <td @click="toggleCompleted">
      <button v-if="todo.completed" :class="statusDisplayClass">Completed</button>
      <button v-else :class="statusDisplayClass">Pending</button>
    </td>
    <td>
      <button @click="deleteTodo" class="delete-btn">Delete</button>
    </td>
  </tr>
</template>

<script setup>
import { onMounted, ref, onUpdated } from 'vue'
const props = defineProps({
  todo: Object
})
const statusDisplayClass = ref('status-display-pending')

const emit = defineEmits(['delete-todo','update-status-complete','update-status-pending'])
const deleteTodo = () => {
  emit('delete-todo', props.todo.id)
}
const toggleCompleted = () => {
  // eslint-disable-next-line vue/no-mutating-props
  props.todo.completed = !props.todo.completed
  if (props.todo.completed) {
    statusDisplayClass.value = 'status-display-completed'
    emit('update-status-complete', props.todo.id)
  } else {
    statusDisplayClass.value = 'status-display-pending'
    emit('update-status-pending', props.todo.id)
  }
}

onMounted(() => {
  if (props.todo.completed) {
    statusDisplayClass.value = 'status-display-completed'
  } else {
    statusDisplayClass.value = 'status-display-pending'
  }
})

onUpdated(() => {
  if (props.todo.completed) {
    statusDisplayClass.value = 'status-display-completed'
  } else {
    statusDisplayClass.value = 'status-display-pending'
  }
})
</script>

<style scoped>
.status-display-pending {
  cursor: pointer;
  margin-left: auto;
  padding: 0.5rem;
  background-color: #ffd52e;
  cursor: pointer;
  font-weight: 700;
  color: white;
  border: #ffd52e solid 3px;
  width: 100px;
  /* text-align: center; */
}
.status-display-completed {
  cursor: pointer;
  margin-left: auto;
  padding: 0.5rem;
  background-color: #00bb00;
  font-weight: 700;
  color: white;
  border: #00bb00 solid 3px;
  width: 100px;
  /* text-align: center; */
}
.todo-text {
  cursor: pointer;
  padding: 0.5rem 0;
  font-weight: 700;
}

.delete-btn {
  cursor: pointer;
  padding: 0.5rem;
  border: #ff0000 solid 3px;
  color: #ff0000;
  font-weight: 700;
  text-align: center;
  margin-left: 1rem;
  background: none;
}
</style>
