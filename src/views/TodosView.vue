<script setup>
import TodoCreator from "../components/TodoCreator.vue";
import { ref } from "vue";
import { uid } from "uid";

const todosList = ref([]);
const createTodo = todo => {
  todosList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false,
  });
};
</script>

<template>
  <main>
    <h1>Create your task</h1>
    <TodoCreator @create-todo="createTodo" />
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
