<script setup>

import { uid } from 'uid'
import { ref, watch, computed } from 'vue'
import { Icon } from '@iconify/vue';
import TodoCreator from '@/components/TodoCreator.vue';
import TodoItem from '@/components/TodoItem.vue';

const todoList = ref([]);

// -------- saving todo list when you refresh the app using WATCH -----------
watch(todoList, () => {
  setTodoListLocalStorage();
}, {
  deep: true, //track changes DEEP within the todoList array
})

const fetchTodoList = () => { // retreive from local storage
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"))
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};
fetchTodoList();

const setTodoListLocalStorage = () => { // save to local storage
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
  // call this function every time we make an update to save the latest version of todo list
  // ==> THIS IS DONE WITH WATCHER (ABOVE)
};

// ------- show message when all todo's completed using COMPUTED --------
const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});
// ------------------------------------------------------------------------


const createTodo = (todo) => {
  todoList.value.push({
    id: uid (),
    todo,
    isCompleted: null,
    isEditing: null,
  });
};

const toggleTodoComplete = (todoPos) => {
  // since it's a TOGGLE, we just set the value of isCompleted opposite of what it was
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};

const deleteTodo = (todoId) => {
  // safely update the todo list 
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
};
</script>

<template>
  <main>
    <!-- make a new todo! -->
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />

    <!-- if we have a todo... -->
    <ul class="todo-list" v-if="todoList.length > 0">
      <!-- fetch todo and index data from todoList
            upon each custom event being triggered by checkbox / pencil / check-circle / trash
            buttons, execute the corresponding function -->
      <TodoItem 
        v-for="(todo, index) in todoList" 
        :todo="todo" 
        :index="index" 
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo" />
    </ul>

    <!-- if we don't have a todo, print a message! -->
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete. Add one!</span>
    </p>

    <!-- if all todo's completed, print a message! -->
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todo's!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}

</style>