<template>
  <div class="container">
    <h1>Todo App</h1>
    <form @submit.prevent="onSubmit">
      <main>
        <label for="newTodo">New Todo</label>
        <input v-model="newTodo" type="text" name="newTodo" />
      </main>
      <button type="submit">Add New Todo</button>
    </form>
    <div class="card__container">
      <div
        class="todo__card"
        v-for="(todo, index) in todos"
        :key="todo.id"
        @click="toggleDone(todo)"
      >
        <p v-if="todo.content" :class="{ done: todo.done, todo__title: true }">
          {{ todo.content }}
        </p>
        <p v-else :class="{ done: todo.done, todo__title: true }">Untitled</p>

        <span class="todo__card--time">Created: {{ todo.time }}</span>
        <button @click="removeTodo(index)">Remove Todo</button>
      </div>
    </div>
    <label>
      <input type="checkbox" name="finishTodos" @click="markAllTodosDone" />
      <span>Mark all ToDos as Done</span>
    </label>
    <button @click="removeAllTodos">Remove All ToDos</button>
  </div>
</template>

<script>
import { ref } from 'vue'
import { v4 as uuid } from 'uuid'

export default {
  setup () {
    const newTodo = ref('')
    const todos = ref([])

    function onSubmit () {
      todos.value.push({
        content: newTodo.value,
        id: uuid(),
        done: false,
        time: new Date().toDateString()
      })
      newTodo.value = ''
    }

    function toggleDone (todo) {
      todo.done = !todo.done
    }

    function removeTodo (index) {
      todos.value.splice(index, 1)
    }

    function markAllTodosDone () {
      todos.value.forEach((todo) => (todo.done = true))
    }

    function removeAllTodos () {
      todos.value = []
    }

    return {
      onSubmit,
      toggleDone,
      removeTodo,
      markAllTodosDone,
      removeAllTodos,
      newTodo,
      todos
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  margin: auto 30px;
}

.todo__card {
  display: flex;
  border: 2px solid midnightblue;
  margin-top: 10px;
  margin-right: 10px;
  width: 100%;
  max-width: 200px;
  flex-direction: column;
  padding: 10px;
  cursor: pointer;
}

.todo__title {
  font-size: 16px;
  font-weight: 600;
  margin: 10px auto;
}

.todo__card--time {
  font-size: 12px;
  font-style: italic;
  text-align: center;
}
.done {
  text-decoration: line-through;
}

.card__container {
  display: flex;
  flex-wrap: wrap;
}

main {
  display: flex;
  flex-direction: column;
}

label {
  font-weight: 600;
  margin-top: 10px;
}

input[type='text'] {
  margin: 10px 0 10px 0;
  padding: 5px;
  max-width: 350px;
  width: 100%;
  font-size: 16px;
}

button {
  padding: 10px;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  color: whitesmoke;
  border: 1px solid midnightblue;
  background-color: midnightblue;
  outline: none;
  border-radius: 5px;
  margin-top: 10px;
}
</style>
