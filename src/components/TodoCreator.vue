<template>
  <div class="input-wrap">
    <input v-model="todoState.content" type="text" />
    <button @click="createTodo()">Create</button>
  </div>
</template>

<script setup>
import { defineEmits, reactive } from "vue";
const emit = defineEmits(["create-todo"]);
const todoState = reactive({ content: "", isInvalid: false, errMsg: "" });

const createTodo = () => {
  todoState.isInvalid = false;
  todoState.errMsg = "";
  if (todoState.content) {
    emit("create-todo", todoState.content);
    todoState.content = "";
  } else {
    todoState.isInvalid = true;
    todoState.errMsg = "Value cannot be an empty string";
  }
};
</script>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  button {
    padding: 8px 16px;
    border: none;
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
