<script setup>

import Todo from "@/components/todos/todo.vue";
import {computed, ref, watch} from "vue";
import AddEditTodo from "@/components/todos/addEditTodo.vue";

const items = ref(JSON.parse(localStorage.getItem('todos')) || [])

watch(items, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
},{
  deep: true
})

const addTodoDialog = ref(false)
const editTodoDialog = ref(false)

const emptyTodo = computed(() => ({
  id: items.value.length + 1,
  title: '',
  description: '',
  completed: false,
}))


const editTodo = ref(null)

const onDelete = (id) => {
  items.value = items.value.filter(item => item.id !== id)
}

const onEdit = (id) => {
  // console.log('onEdit', id)
  editTodo.value = items.value.find(item => item.id === id)
  editTodoDialog.value = true
}

const onAdd = (todo) => {
  // console.log('onAdd', todo)
  items.value.push(todo)
  addTodoDialog.value = false
}

const onSaveEdit = (todo) => {
  // console.log('onSaveEdit', todo)
  items.value = items.value.map(item => item.id === todo.id ? todo : item)
  editTodoDialog.value = false
}

const updateCompletion = (id,value) => {
  // console.log('updateCompletion', value)
  items.value = items.value.map(item => item.id === id ? {...item, completed: value} : item)
  console.warn(items.value)
}
</script>

<template>
  <div class="about h-100 overflow-auto pa-2">
    <div class="d-flex align-center justify-space-between">
      <h1 class="text-h5 text-primary">Todos ({{items.length}})</h1>
      <VBtn
        size="small"
        color="primary"
        @click="addTodoDialog = true">Add Todo</VBtn>
    </div>

    <TransitionGroup v-if="items.length" name="list">
      <Todo v-for="(item,index) in items" :key="item.id" :todo="item" :serial="index + 1" @delete="onDelete" @edit="onEdit"
            @update="updateCompletion"
      />
    </TransitionGroup>
    <div v-else class="d-flex justify-center mt-3">
      No Todos Found.
    </div>

    <AddEditTodo v-if="addTodoDialog" :dialog="addTodoDialog" :todo="emptyTodo" @close="addTodoDialog = false" @save="onAdd" type="Add"/>
    <AddEditTodo v-if="editTodoDialog" :dialog="editTodoDialog" :todo="editTodo" @close="editTodoDialog = false" @save="onSaveEdit" type="Edit"/>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
