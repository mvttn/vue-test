<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import ChildComp from './ChildComp.vue'

const titleClass = ref('title')

const count = ref(0)

function increment() {
  return count.value++
}

const text = ref('')

// function onInput(e) {
//   text.value = e.target.value
// }

const awesome = ref(true)

function toggle() {
  awesome.value = awesome.value ? false : true
}
// List rendering
// give each todo a unique id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  // ...
  const newItem = { id: id++, text: newTodo.value, done: false }
  todos.value.push(newItem)
  newTodo.value = ''
}

function removeTodo(todo) {
  // ...
  todos.value = todos.value.filter((item) => item !== todo)
}

const hideCompleted = ref(false)

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((item) => !item.done) : todos.value
})

const pElementRef = ref(null)

onMounted(() => {
  pElementRef.value.textContent = 'hello worasdioask'
})

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${todoId.value}`)
  todoData.value = await res.json()
}

fetchData()

watch(todoId, fetchData)
const greeting = ref('hello from parentjs')
const childMsg = ref('No child msg yet')
const msg = ref('frparentom ')
</script>

<template>
  <div>
    <!-- Attribute bindings -->
    <h1 :class="titleClass">Make me red</h1>
    <!-- Event listeners -->
    <button @click="increment">Count is: {{ count }}</button>
    <!-- Form bindings -->
    <input v-model="text" placeholder="Type here..." />
    <p>The text is: {{ text }}</p>
    <!-- Conditional rendering -->
    <button @click="toggle">Toggle</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
    <!-- List rendering -->
    <form @submit.prevent="addTodo">
      <input v-model="newTodo" required placeholder="new todo" />
      <button>Add Todo</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.text }}
        <button @click="removeTodo(todo)">X</button>
      </li>
    </ul>
    <!-- Computed property -->
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)">X</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
    <!-- Lifestyle/Template refs -->
    <p ref="pElementRef">Hello</p>
    <!-- Watchers -->
    <p>Todo id: {{ todoId }}</p>
    <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
    <p v-if="!todoData">Loading...</p>
    <pre v-else>{{ todoData }}</pre>
    <!-- Components -->
    <ChildComp />
    <!-- Props -->
    <ChildComp :msg="greeting" />
    <!-- Emits -->
    <ChildComp @response="(msg) => (childMsg = msg)" />
    <p>{{ childMsg }}</p>
    <!-- Slots -->
    <ChildComp>Message: {{ msg }}</ChildComp>
  </div>
</template>

<style>
div {
  display: flex;
  flex-direction: column;
}
.title {
  color: red;
}
.done {
  text-decoration: line-through;
}
</style>
