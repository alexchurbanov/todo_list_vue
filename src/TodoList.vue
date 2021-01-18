<template>
  <div id="app">
    <TodoForm :todos.sync="todos"/>
    <TodoItems :todos.sync="todos"/>
  </div>
</template>

<script>
import TodoItems from "@/components/TodoItems.vue";
import TodoForm from "@/components/TodoForm";

export default {
  name: "App",
  data: function () {
    return {
      todos: []
    };
  },
  components: {
    TodoItems,
    TodoForm
  }
}
</script>

<style>
button {
  padding: 16px;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
  outline: none;
  background: linear-gradient(90deg, #5d0cff 0%, #9b00fa 100%);
  color: #fff;
  text-transform: capitalize;
}
#app {
  display: flex;
  flex-direction: column;
  justify-content: start;
  width: 520px;
  min-height: 600px;
  background: #161a2b;
  text-align: center;
  margin: 0 auto;
  border-radius: 10px;
  padding-bottom: 32px;
}
body {
  background: linear-gradient(90deg, #5d0cff 0%, #9b00fa 100%);
}
</style>
