<template>
  <form class="todo-form">
    <label>
      <input class="todo-input" type="text" v-model="todo_name" placeholder="Input Todo name"/>
    </label>
    <button class="todo-form-button" type="button" @click="addTodo">
      <span>Add Todo</span>
    </button>
  </form>
</template>

<script>
export default {
  name: "TodoForm",
  props: {
    todos: Array,
  },
  data: function () {
    return {
      todo_name: ""
    };
  },
  methods: {
    addTodo: function () {
      if (!this.todo_name) {
        alert('You must write something!');
        return;
      }
      this.todos.push({id: Math.floor(Math.random() * Math.floor(10000)), name: this.todo_name});
      this.todo_name = '';
    }
  }
};
</script>

<style>

.todo-form {
  padding: 0 5px;
  margin-top: 20px;
}

.todo-input {
  padding: 14px 32px 14px 16px;
  border-radius: 4px 0 0 4px;
  border: 2px solid #5d0cff;
  outline: none;
  width: 320px;
  background: transparent;
  color: #fff;
}

.todo-form-button {
  margin-left: 5px;
  border-radius: 4px;
  padding: 10px;
}
</style>