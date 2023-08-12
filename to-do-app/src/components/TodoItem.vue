<!-- eslint-disable vue/no-mutating-props -->
<template>
  <tr class="item-list">
    <td>
      <span @click="toggleCompleted" class="todo-text">{{ todo.text }}</span>
    </td>
    <td @click="toggleCompleted" :class="statusDisplayClass">
      <span v-if="todo.completed" style="font-weight: 700; color: white">Completed</span>
      <span v-else style="font-weight: 700; color: white">Pending</span>
    </td>
  </tr>
</template>

<script setup>
import { ref } from 'vue'
const props = defineProps({
  todo: Object
})
const statusDisplayClass = ref('status-display-pending')

const toggleCompleted = () => {
  // eslint-disable-next-line vue/no-mutating-props
  props.todo.completed = !props.todo.completed
  if (props.todo.completed) {
    statusDisplayClass.value = 'status-display-completed'
  } else {
    statusDisplayClass.value = 'status-display-pending'
  }
}
</script>

<style scoped>
.status-display-pending {
  cursor: pointer;
  /* margin-left: auto; */
  padding: 0.5rem;
  background-color: #ffd52e;
  cursor: pointer;
  font-weight: 700;
  /* text-align: center; */
}
.status-display-completed {
  cursor: pointer;
  /* margin-left: auto; */
  padding: 0.5rem;
  background-color: #00bb00;
  font-weight: 700;
  /* text-align: center; */
}
.todo-text {
  cursor: pointer;
  padding: 0.5rem 0;
  font-weight: 700;
}
</style>
