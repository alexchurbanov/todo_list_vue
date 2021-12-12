<template>
  <div class="todo-items">
    <transition-group name="flip-list" tag="div">
      <div class="todo-wrapper" v-for="(todo, index) in todo_list" :key="todo.id">
        <transition name="fade" mode="out-in">
          <div class="todo-item" v-if="!todo.edit" @click="checkTodo(todo, index)" :class="{checked: todo.checked}">
            <span class="todo-name">{{ todo.name }}</span>
            <div class="todo-buttons">
              <button class="todo-remove-btn" type="button" @click.stop="removeTodo(index)">
                <span>Delete Todo</span>
              </button>
              <button class="todo-edit-btn" type="button" @click.stop="toggleEdit(todo, index)"
                      :disabled="todo.checked">
                <span>Edit Todo</span>
              </button>
            </div>
          </div>
          <form class="todo-edit-form" v-if="todo.edit">
            <label>
              <input class="todo-input" type="text" v-model="todo.name"/>
            </label>
            <div class="todo-buttons">
              <button class="todo-form-button" type="button" @click="editTodo(todo, index)">
                <span>Ok</span>
              </button>
              <button class="todo-remove-btn" type="button" @click="cancelEdit(todo, index)">
                <span>Cancel</span>
              </button>
            </div>
          </form>
        </transition>
      </div>
    </transition-group>
  </div>
</template>

<script>

export default {
  name: "TodoItems",
  props: {
    todos: Array,
  },
  data: function () {
    return {
      todo_list: this.todos,
    };
  },
  methods: {
    removeTodo: function (index) {
      this.todo_list.splice(index, 1);
    },
    toggleEdit: function (todo, index) {
      this.$set(this.todo_list, index, {...todo, old_name: todo.name, edit: true});
    },
    editTodo: function (todo, index) {
      if (!todo.name) return alert('You must write something!');
      this.$set(this.todo_list, index, {...todo, edit: false, old_name: ''});
    },
    cancelEdit: function (todo, index) {
      this.$set(this.todo_list, index, {...todo, name: todo.old_name, edit: false, old_name: ''});
    },
    checkTodo: function (todo, index) {
      this.$set(this.todo_list, index, {...todo, checked: !todo.checked});
      this.todo_list.sort((a, b) => {
        if (a.checked === b.checked) return 0;
        if (a.checked) return 1;
        if (b.checked) return -1;
      })
    }
  }
};
</script>

<style>
.todo-items {
  margin-top: 10px;
}

.todo-item {
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
  border-radius: 4px;
  padding: 10px;
  background: linear-gradient(90deg, #680606 0%, #990000 100%);
  z-index: 1;
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

.todo-edit-form {
  margin: 10px;
  display: flex;
}

.checked {
  background: linear-gradient(90deg, #827c7c 0%, #535050 100%);
  position: relative;
}

.checked::after {
  position: absolute;
  top: calc(50% - .5 * .125em);
  right: 0;
  width: 500px;
  height: .125em;
  border-radius: 0 .125em .125em 0;
  background: #7d0c1f;
  content: '';
}

.todo-edit-btn:disabled {
  background: linear-gradient(90deg, #b6a6a6 0%, #2f2e2e 100%);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .22s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.flip-list-move {
  transition: transform 0.5s;
}

.flip-list-enter-active, .flip-list-leave-active {
  transition: all 0.3s;
}

.flip-list-enter, .flip-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
