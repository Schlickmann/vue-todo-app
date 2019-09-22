<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

import axios from "axios";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    async deleteTodo(id) {
      try {
        const response = await axios.delete(
          `https://jsonplaceholder.typicode.com/todos/${id}`
        );

        this.todos = this.todos.filter(todo => todo.id !== id);
      } catch (error) {
        console.log(error);
      }
    },
    async addTodo(newTodo) {
      const { title, completed } = newTodo;

      try {
        const response = await axios.post(
          "https://jsonplaceholder.typicode.com/todos",
          {
            title,
            completed
          }
        );

        this.todos = [...this.todos, response.data];
      } catch (error) {
        console.log(error);
      }
    }
  },
  async created() {
    try {
      const response = await axios.get(
        "https://jsonplaceholder.typicode.com/todos?_limit=10"
      );
      this.todos = response.data;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
input:focus {
  outline: 0;
}

.btn {
  display: inline-block;
  border: none;
  background-color: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background-color: #666;
}
</style>
