<template>
  <div id="app">
    <h1>{{uwu2}}</h1>
    <input type="text" v-model="name" placeholder="Todo name" />
    <input type="text" v-model="description" placeholder="Todo description" />
    <button v-on:click="createTodo">Create Todo</button>
    <div v-for="item in todos" :key="item.id">
      <h3>{{ item.name }}</h3>
      <p>{{ item.description }}</p>
    </div>
  </div>
</template>

<script>
  import { API } from 'aws-amplify';
  import { createTodo } from './graphql/mutations';
  import { listTodos } from './graphql/queries';

  export default {
    async created() {
      this.getTodos();
    },
    data() {
      return {
        name: '',
        description: '',
        todos: [],
        uwu: 'asdfasdf'
      };
    },
    methods: {
      async createTodo() {
        const { name, description } = this;
        if (!name || !description) return;
        const todo = { name, description };
        this.todos = [...this.todos, todo];
        await API.graphql({
          query: createTodo,
          variables: { input: todo }
        });
        this.name = '';
        this.description = '';
      },
      async getTodos() {
        const todos = await API.graphql({
          query: listTodos
        });
        this.todos = todos.data.listTodos.items;
      }
    }
  };
  uwu2="12341234"
</script>