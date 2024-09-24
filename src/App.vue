<script setup>
import { ref, computed } from 'vue';

const todos = ref([]);
const newTodo = ref("");
const hideCompleted = ref(false);

const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  });
  newTodo.value = "";
};

const sortedTodos = computed(() => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1);
  if (hideCompleted.value) {
    return sortedTodos.filter(t => !t.completed);
  }
  return sortedTodos;
});

const remainingTodos = computed(() => {
  return todos.value.filter(t => !t.completed).length;
});
</script>

<style>
body {
  background-color: #e0f7fa; 
  color: #004d40; 
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Prendre toute la hauteur de la fenêtre */
  width: 100%; /* Assurer que le body prend toute la largeur */
}

.container {
  width: 100%;
  max-width: 600px; 
  background-color: #ffffff; 
  padding: 15px; 
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
 /* Cela fonctionne pour centrer horizontalement */
}



.container:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.logo {
  text-align: center;
  font-size: 2.5em; 
  color: #00695c; 
  margin-bottom: 15px; 
  font-weight: bold;
  font-family: 'Arial', sans-serif;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1); 
}

form {
  display: flex;
  align-items: center;
  margin-bottom: 15px; 
}

input[type="text"] {
  flex: 1;
  padding: 8px; 
  border: 1px solid #00796b; 
  border-radius: 4px;
  font-size: 0.9em; 
  transition: border-color 0.3s ease;
}

input[type="text"]:focus {
  border-color: #004d40; 
}

button {
  padding: 8px 15px; 
  background-color: #00796b; 
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9em; 
  margin-left: 5px; 
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #004d40; 
  transform: translateY(-2px); /* Lift effect */
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 8px; 
  border-bottom: 1px solid #00796b; 
  display: flex;
  align-items: center;
  font-size: 0.9em; 
  transition: background-color 0.3s ease, transform 0.2s ease;
}

li:hover {
  background-color: #b2ebf2; 
  transform: translateX(2px); /* Slight slide effect */
}

.completed {
  opacity: 0.6;
  text-decoration: line-through;
}

.toggle-completed {
  margin-top: 15px; 
  display: flex;
  align-items: center;
}
</style>

<template>
  <div class="container">
    <h1 class="logo">DJtodo</h1>
    <form @submit.prevent="addTodo">
      <fieldset role="group">
        <input type="text" v-model="newTodo" placeholder="Neue Aufgabe hinzufügen">
        <button :disabled="newTodo.length === 0">Hinzufügen</button>
      </fieldset>
    </form>
    <div v-if="todos.length === 0">Keine Aufgaben zu erledigen</div>
    <div v-else>
      <ul>
        <li v-for="todo in sortedTodos" :key="todo.date" :class="{ completed: todo.completed }">
          <label>
            <input type="checkbox" v-model="todo.completed">
            {{ todo.title }}
          </label>
        </li>
      </ul>
      <div class="toggle-completed">
        <label>
          <input type="checkbox" v-model="hideCompleted">
          Abgeschlossene Aufgaben verbergen
        </label>
      </div>
      <p v-if="remainingTodos > 0">{{ remainingTodos }} Aufgabe(n) übrig</p>
    </div>
  </div>
</template>
