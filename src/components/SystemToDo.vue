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
  <div class="choiseTodolist">
    <div class="aktivitas">
      <a href="">Aktifitas</a>
    </div>
    <div class="filteraktivitas">
      <a href="">Aktifitas Belum Selesai</a>
    </div>
  </div>

  <div>
    <h3>Input Aktivitas :</h3>
  </div>

  <div class="input">
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" required placeholder="Tambahkan Tugas" />
      <button>Tambahkan</button>
    </form>
  </div>

  <div class="activity">
    <h3>Activity</h3>
  </div>

  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <div class="hideCompleted">
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "Tampilkan Semua" : "Tugas Yang Belum Selesai" }}
    </button>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
