<template>
  <div
    @click="$emit('toggle-reminder', todo.id)"
    :class="[todo.reminder ? 'reminder' : '', 'todo']"
  >
    <h3>
      {{ todo.text }}
      <i @click="$emit('delete-todo', todo.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ todo.day }}</p>
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: {
    todo: Object,
  },
};
</script>

<style scope>

.fas {
  color: red;
}

.todo {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.todo.reminder {
  border-left: 5px solid green;
}

.todo h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

</style>