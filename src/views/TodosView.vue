<script setup>
import TodoCreator from "../components/TodoCreator.vue";
import TodoItem from "../components/TodoItem.vue";
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";

const todosList = ref([]);
const createTodo = todo => {
  todosList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false,
  });
};

const getTodos = () => {
  const todos = JSON.parse(localStorage.getItem("todos"));
  if (todos) {
    todosList.value = todos;
  }
};
getTodos();
const setTodosToLocalStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todosList.value));
};

const toggleComplete = todoIndex => {
  todosList.value[todoIndex].isCompleted =
    !todosList.value[todoIndex].isCompleted;
};

const editTodo = todoIndex => {
  todosList.value[todoIndex].isEditing = !todosList.value[todoIndex].isEditing;
};

const updateTodo = (value, todoIndex) => {
  todosList.value[todoIndex].todo = value;
};

const deleteTodo = todo => {
  todosList.value = todosList.value.filter(
    todoFilter => todoFilter.id !== todo.id
  );
};

watch(
  todosList,
  () => {
    setTodosToLocalStorage();
  },
  { deep: true }
);

const areAllTasksCompleted = computed(() => {
  return todosList.value.every(todo => todo.isCompleted);
});
</script>

<template>
  <main>
    <h1>Create your task</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todosList.length">
      <TodoItem
        v-for="(todo, index) in todosList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleComplete"
        @edit-todo="editTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>

    <p v-if="areAllTasksCompleted && todosList.length" class="todos-msg">
      You have finished your tasks
    </p>
  </main>
</template>

<style scoped lang="scss">
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
