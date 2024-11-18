<template>    
    <h2>Ajouter une tâche</h2>
    <form @submit.prevent="pushTodo">
        <label>Nom de la tâche</label>
        <input type="text" v-model="newTodo.name">

        <label>Tâche effectuée ?</label>
        <input type="checkbox" v-model="newTodo.completed">

        <button :disabled="newTodo.name.length === 0">Ajouter la tâche</button>
    </form>


    <h1 v-if="todoList.length === 0">Il n'y a aucune tâche à faire</h1>
    <h1 v-else>Voici la liste des tâches à faire</h1>

    <div>
        <label>Afficher seulements les tâches complétées</label>
        <input type="checkbox" v-model="hideCompleted">

        <ul v-for="todo in sortedTodos" :key="todo.date">
            <li :style="{'text-decoration': todo.completed ? 'line-through' : 'none'}">
                {{ todo.name }}
            </li>
            <li><Checkbox label="Tâche effectué ?" v-model="todo.completed" /></li>
            <li>{{ new Date(todo.date) }}</li>
        </ul>        
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import Checkbox from './Checkbox.vue';

const hideCompleted = ref(false);

const todoList = ref([{
    name: 'Tache n°1',
    completed: false,
    date: 1020302103 
}]);

const newTodo = ref({
    name: '',
    completed: false,
    date: null
})

const pushTodo = () => {
    todoList.value.push({...newTodo.value, date: Date.now()});
    resetNewTodo()
}

const resetNewTodo = () => {
    newTodo.value.name = '';
    newTodo.value.completed = false;
    newTodo.value.date = null;
}

const sortedTodos = computed(() => {
    const sortedTodos = todoList.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
    
    if (hideCompleted.value === true) {
        return sortedTodos.filter(t => !t.completed)
    }

    return sortedTodos
})
</script>