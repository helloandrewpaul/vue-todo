<template>
  <div class="container">
    <Header
      @toggle-add-todo="toggleAddTodo"
      title="To Do List"
      :showAddTodo="showAddTodo"
    />
    <div v-show="showAddTodo"><AddTodo @add-todo="addTodo" /></div>
    <Todos
      @toggle-reminder="toggleReminder"
      @delete-todo="deleteTodo"
      :todos="todos"
    />
    <router-view></router-view>
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header";
import Todos from "./components/Todos";
import AddTodo from "./components/AddTodo";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    Todos,
    AddTodo,
    Footer,
  },
  data() {
    return {
      todos: [],
      showAddTodo: false,
    };
  },
  methods: {
    toggleAddTodo() {
      this.showAddTodo = !this.showAddTodo;
    },
    async addTodo(todo) {
      const res = await fetch("api/todos", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(todo),
      });
      const data = await res.json();
      this.todos = [...this.todos, data];
    },
    async deleteTodo(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/todos/${id}`, { method: "DELETE" });

        res.status === 200
          ? (this.todos = this.todos.filter((todo) => todo.id !== id))
          : alert("Error deleting todo");
      }
    },
    async toggleReminder(id) {
      const todoToToggle = await this.fetchTodo(id);
      const updateTodo = { ...todoToToggle, reminder: !todoToToggle.reminder };

      const res = await fetch(`api/todos/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updateTodo),
      });

      const data = await res.json();

      this.todos = this.todos.map((todo) =>
        todo.id === id ? { ...todo, reminder: data.reminder } : todo
      );
    },
    async fetchTodos() {
      const res = await fetch("api/todos");

      const data = await res.json();

      return data;
    },
    async fetchTodo(id) {
      const res = await fetch(`api/todos/${id}`);

      const data = await res.json();

      return data;
    },
  },

async created() {
    this.todos = await this.fetchTodos();
  },
};
</script>

<style>

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}

</style>
