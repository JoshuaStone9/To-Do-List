<template>
    <h1>To Do List</h1>

    <form @submit.prevent="addTask">
        <input v-model="newTask" placeholder="Add A Task Here..."/>
        <button>Add</button>

    <ul>
        <li v-for="(t, i) in tasks" :key="t.id">
            <label>
                <input type="checkbox" v-model="t.done" />
                <span :class ="{ done: t.done }">{{ t.text}}</span>
            </label>
            <button @click="removeTask(i)">X</button>
        </li>
    </ul>

    <p v-if="remaining === 0">All done</p>
    <p v-else>{{ remaining }} left</p>
</template>

<script setup>
import { ref, vomputed, watch } from 'vue'

// reactive state 
const newTask = ref('')
const tasks = ref(JSON.parse(localStorage.getItem('tasks') || '[]' ))

// derived state
const remaining = computed(() => tasks.value.filter(t => !t.done).length)

// actions

function addTask() {
    const text = newTask.value.trim()
    if(!text) return
    tasks.value.push({ id: crypto.randomUUID(), text, done: false })
    newTask.value = ''
}