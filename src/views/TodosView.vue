<template>
  <ul class="todos">
    <li
      v-for="todo in todos"
      :key="todo.id"
      :class="{ 'todo-completed': todo.is_complete }"
      @click="onSelect(todo)"
    >
      <span>{{ todo.text }}</span>
    </li>
  </ul>
  <button v-if="!isAdding" @click="showAddForm()">Add</button>
  <input
    type="text"
    v-model="todoText"
    placeholder="Description"
    v-if="isAdding"
    v-on:keyup.enter="addTodo(todoText)"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface ITodo {
  id: number;
  text: string;
  created_at: string;
  is_complete: boolean;
  selected?: boolean;
}

export default defineComponent({
  data() {
    let isAdding = false;
    let todoText = "";
    let todos: ITodo[] = [
      {
        id: 1,
        text: "Learn about Polymer",
        created_at: "Mon Apr 26 06:01:55 +0000 2015",
        is_complete: true,
      },
      {
        id: 2,
        text: "Watch Pluralsight course on Docker",
        created_at: "Tue Mar 02 07:01:55 +0000 2015",
        is_complete: true,
        selected: true,
      },
      {
        id: 3,
        text: "Complete presentation prep for Aurelia presentation",
        created_at: "Wed Mar 05 10:01:55 +0000 2015",
        is_complete: false,
      },
    ];
    return { todos, isAdding, todoText };
  },
  mounted() {
    console.log("component mounted");
  },
  unmounted() {
    console.log("component unmounted");
  },
  methods: {
    onSelect(todo: ITodo) {
      todo.is_complete = !todo.is_complete;
    },
    showAddForm() {
      this.isAdding = !this.isAdding;
    },
    addTodo(todoText: string) {
      let todo: ITodo = {
        id: this.todos.length * 1,
        text: todoText,
        created_at: Date.now() + "",
        is_complete: false,
      };
      this.todos.push(todo);
      this.isAdding = !this.isAdding;
    },
  },
});
</script>

<style scoped>
.hidden {
  display: none;
}
.todos {
  list-style: none;
  text-align: left;
}
.todos > li:hover {
  opacity: 0.6;
}
.todo-completed {
  color: green;
}
.todo-completed:before {
  position: absolute;
  margin-left: -1em;
  content: "\2713";
}
</style>
