<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input v-model="newTodo" type="text" placeholder="Tache a effecruer">
      <button :disabled="newTodo.length===0">Ajouter</button>
    </fieldset>
  </form>
  
  <div v-if="todos.length===0">Vous n'avez pas de taches a faire</div>
  <div v-else>
    <ul>
      <li v-for="todo in sortedTodos" :key="todo.date" :class="{completed: todo.completed}">
        <label>
          <input type="checkbox" v-model="todo.completed">
          {{ todo.title }}
        </label>
      </li>
    </ul>
    <label for="">
      <input type="checkbox" v-model="hideCompleted">
      Masquer les taches completees
    </label>
    <p v-if="remainingTodos > 0">
      {{ remainingTodos }} tache{{ remainingTodos > 1 ? 's' : '' }} a faire
    </p>
  </div>
</template>

<script setup>
import {computed, ref} from 'vue'

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([{
  title:'tache de test',
  completed:true,
  date:1
},{
  title:'tache a faire',
  completed:false,
  date:2
}
])

const addTodo = () => {
  todos.value.push ({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}

const sortedTodos = computed ( () => {
  const sortedTodos = todos.value.toSorted((a,b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
})
const remainingTodos = computed (() =>{
  return todos.value.filter(t => t.completed === false).length
}) 

</script>

<style>
.completed {
  opacity:.5 ;
  text-decoration: line-through;
}
</style>
