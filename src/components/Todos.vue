<template>
  <div :key="todo.id" v-for="todo in todos">
    <Todo
      @toggle-reminder="$emit('toggle-reminder', todo.id)"
      @delete-todo="$emit('delete-todo', todo.id)"
      :todo="todo"
    />
  </div>
</template>

<script>
import Todo from "./Todo";
export default {
  name: "Todos",
  props: {
    todos: Array,
  },
  components: {
    Todo,
  },
  emits: ["delete-todo", "toggle-reminder"],
};
</script>