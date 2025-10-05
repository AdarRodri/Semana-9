<template>
  <div class="container">
    <h1 class="title">Gestor de tareas</h1>

    <!-- Input de nueva tarea -->
    <div class="task-input">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Ingresa el nombre de la tarea"
      />
      <button @click="addTask">Agregar</button>
    </div>

    <hr />

    <!-- Si no hay tareas -->
    <div v-if="tasks.length === 0" class="empty">
      <p>No hay tareas registradas</p>
    </div>

    <!-- Si hay tareas -->
    <div v-else class="board">
      <!-- To Do -->
      <div class="column todo">
        <h2>To do</h2>
        <div
          v-for="(task, index) in tasks.filter(t => t.status === 'todo')"
          :key="index"
          class="task-card"
        >
          <span>{{ task.name }}</span>
          <button @click="moveTask(task, 'doing')">➡️</button>
        </div>
      </div>

      <!-- Doing -->
      <div class="column doing">
        <h2>Doing</h2>
        <div
          v-for="(task, index) in tasks.filter(t => t.status === 'doing')"
          :key="index"
          class="task-card"
        >
          <button @click="moveTask(task, 'todo')">⬅️</button>
          <span>{{ task.name }}</span>
          <button @click="moveTask(task, 'done')">➡️</button>
        </div>
      </div>

      <!-- Done -->
      <div class="column done">
        <h2>Done</h2>
        <div
          v-for="(task, index) in tasks.filter(t => t.status === 'done')"
          :key="index"
          class="task-card"
        >
          <button @click="moveTask(task, 'doing')">⬅️</button>
          <span>{{ task.name }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue"

const newTask = ref("")
const tasks = ref([])

const addTask = () => {
  if (newTask.value.trim() === "") return
  tasks.value.push({ name: newTask.value, status: "todo" })
  newTask.value = ""
}

const moveTask = (task, newStatus) => {
  task.status = newStatus
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: auto;
  text-align: center;
  font-family: Arial, sans-serif;
}

.title {
  margin-bottom: 20px;
  color: #333;
}

.task-input {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 15px;
}

.task-input input {
  padding: 8px;
  width: 250px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

.task-input button {
  padding: 8px 12px;
  background: #007bff;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
}

.task-input button:hover {
  background: #0056b3;
}

.empty {
  font-style: italic;
  color: gray;
  margin-top: 20px;
}

.board {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.column {
  flex: 1;
  margin: 0 10px;
  padding: 15px;
  border-radius: 12px;
  background: #f9f9f9;
  min-height: 250px;
}

.column h2 {
  margin-bottom: 15px;
}

.todo {
  border: 2px solid #14171a;
}

.doing {
  border: 2px solid #084374;
}

.done {
  border: 2px solid #09a12f;
}

.task-card {
  background: white;
  border-radius: 8px;
  padding: 10px;
  margin: 8px 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0px 2px 4px rgba(0,0,0,0.1);
}

.task-card span {
  flex: 1;
  text-align: center;
}

.task-card button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
}
</style>
