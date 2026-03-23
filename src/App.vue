<script setup>
import { ref } from 'vue'

// 1. Dito natin ise-save ang mga tasks
const newTask = ref('')
const todos = ref([
  { id: 1, text: 'Matuto ng Vue.js', done: true },
  { id: 2, text: 'Gumawa ng App', done: false }
])

// 2. Function para magdagdag ng task
function addTodo() {
  if (newTask.value.trim() === '') return // Wag mag-add kung empty
  
  todos.value.push({
    id: Date.now(),
    text: newTask.value,
    done: false
  })
  
  newTask.value = '' // Clear ang input box pagkatapos mag-add
}

// 3. Function para mag-delete
function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <div class="todo-app">
    <h1>📝 My Tasks</h1>

    <form @submit.prevent="addTodo">
      <input v-model="newTask" placeholder="Ano ang gagawin mo?">
      <button>Add Task</button>
    </form>

    <TransitionGroup name="list" tag="ul">
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.done">
        <span :class="{ finished: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)" class="del-btn">X</button>
      </li>
    </TransitionGroup>

    <p v-if="todos.length === 0">Yehey! Tapos na lahat. 🥳</p>
  </div>
</template>

<style scoped>
.todo-app { max-width: 400px; margin: 50px auto; font-family: sans-serif; }
form { display: flex; gap: 10px; margin-bottom: 20px; }
input[type="text"] { flex: 1; padding: 8px; }
ul { list-style: none; padding: 0; }
li { 
  display: flex; 
  align-items: center; 
  gap: 10px; 
  padding: 10px; 
  border-bottom: 1px solid #eee; 
}
.finished { text-decoration: line-through; color: gray; }
.del-btn { background: #ff4d4d; color: white; border: none; padding: 2px 8px; cursor: pointer; border-radius: 4px; }


/* -------------------------------------animation  */
/* 1. Animation habang pumapasok (Enter) at lumalabas (Leave) */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

/* 2. Start state (pag kapapasok lang) at End state (pag paalis na) */
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

/* 3. Para hindi "tumatalon" yung ibang items pag may tinanggal ka */
.list-move {
  transition: transform 0.5s ease;
}
</style>