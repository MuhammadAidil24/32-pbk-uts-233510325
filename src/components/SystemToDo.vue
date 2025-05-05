<script setup>
import { ref, computed, onMounted, watch } from "vue";

let id = 0;

const newTodo = ref("");
const hideCompleted = ref(false);
const todos = ref([]);

// Muat data dari localStorage saat komponen di-mount
onMounted(() => {
  const savedTodos = localStorage.getItem("todos");
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos);
    id = todos.value.length ? Math.max(...todos.value.map((t) => t.id)) + 1 : 0;
  }
});

// Simpan data ke localStorage setiap kali todos berubah
watch(
  todos,
  (newTodos) => {
    localStorage.setItem("todos", JSON.stringify(newTodos));
  },
  { deep: true }
);

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
  <div class="input">
    <form @submit.prevent="addTodo">
      <h3>Tambah Aktivitas</h3>
      <input v-model="newTodo" required placeholder="Tambahkan Tugas" />
      <button>Tambahkan</button>
    </form>
  </div>

  <div class="activity">
    <h3>Aktivitas</h3>
    <div class="todoList">
      <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button @click="removeTodo(todo)">hapus</button>
        </li>
      </ul>
    </div>
    <div class="hideCompleted">
      <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? "Tampilkan Semua" : "Tugas Yang Belum Selesai" }}
      </button>
    </div>
  </div>
</template>

<style scoped></style>
