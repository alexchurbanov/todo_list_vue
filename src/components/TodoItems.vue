<template>
  <div class="todo-items">
    <div class="todo" v-for="(todo, index) in todo_list" :key="todo.id" v-show="!edit_items">
      <span class="todo-name">{{ todo.name }}</span>
      <div class="todo-buttons">
        <button class="todo-remove-btn" type="button" @click="removeTodo(todo)">
          <span>Delete Todo</span>
        </button>
        <button class="todo-edit-btn" type="button" @click="edit_items = {item: todo, index: index}">
          <span>Edit Todo</span>
        </button>
      </div>
    </div>
    <form class="todo-form" v-if="edit_items">
      <label>
        <input class="todo-input" type="text" v-model="new_name" :placeholder="edit_items.item.name"/>
      </label>
      <button class="todo-form-button" type="button" @click="editTodo">
        <span>Ok</span>
      </button>
      <button class="todo-remove-btn" type="button" @click="cancelEdit">
        <span>Cancel</span>
      </button>
    </form>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  name: "TodoItems",
  props: {
    todos: Array,
  },
  data: function () {
    return {
      todo_list: this.todos,
      edit_items: null,
      new_name: ''
    };
  },
  methods: {
    removeTodo: function (todo) {
      this.todo_list = _.remove(this.todo_list, item => {
        return item.id !== todo.id
      });
      this.$emit('update:todos', this.todo_list);
    },
    editTodo: function () {
      if (!this.new_name) {
        alert('You must write something!');
        return;
      }
      this.$set(this.todo_list, this.edit_items.index, {id: this.edit_items.item.id, name: this.new_name});
      this.$emit('update:todos', this.todo_list);
      this.edit_items = null;
      this.new_name = '';
    },
    cancelEdit: function () {
      this.edit_items = null;
      this.new_name = '';
    }
  }
};
</script>

<style>
.todo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  background: linear-gradient(90deg, #ff7614 0%, #ff5411 100%);
  padding: 16px;
  border-radius: 5px;
  width: 90%;
  font-weight: 400;
  margin: 4px auto 10px;
}

.todo-remove-btn {
  margin-left: 5px;
  border-radius: 4px;
  padding: 10px;
  background: linear-gradient(90deg, #680606 0%, #990000 100%);
}

.todo-edit-btn {
  margin-left: 5px;
  border-radius: 4px;
  padding: 10px;
}

.todo-buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 10px;
}
</style>
