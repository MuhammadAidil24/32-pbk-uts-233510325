<script setup>
import { ref } from "vue";
import { computed } from "vue";

let id = 0;

const newTodo = ref("");
const hideCompleted = ref(false);
const todos = ref([]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});

function addTodo() {
  if (newTodo.value.trim() !== "") {
    todos.value.push({ id: id++, text: newTodo.value, done: false });
    newTodo.value = "";
  }
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}
</script>

<template>
  <div class="container">
    <h1>TO DO LIST.</h1>
  </div>

  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="Tambahkan Tugas" />
    <button>Tambahkan</button>
  </form>

  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? "Show all" : "Tugas Yang Belum Selesai" }}
  </button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}

.container {
  max-width: 600px;
  margin: 2rem auto;
  font-family: sans-serif;
}
</style>
