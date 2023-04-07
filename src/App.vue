<template>
  <div class="main">
    <InputCustom v-model="newTodo" @addElem="addTodo" />
    <div class="todosList">
      <div v-if="todos.length === 0">
        There is nothing to show here, add some TO-DO items to the list.
      </div>
      <div v-if="todos.length > 0">
        Sort by:
        <select v-model="sortBy">
          <option selected value="des">Newest</option>
          <option value="asc">Oldest</option>
        </select>
      </div>
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :content="todo"
        :timestamp="Date.now()"
        :idx="index"
        @deleteItem="(idx) => removeTodo(idx)"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from "./components/TodoItem.vue";
import InputCustom from "./components/InputCustom.vue";
export default {
  data() {
    return {
      sortBy: "des",
      newTodo: "",
      todos: [],
    };
  },
  methods: {
    addTodo: function () {
      if (this.sortBy === "des") {
        this.newTodo.trim() && this.todos.unshift(this.newTodo.trim());
      } else {
        this.newTodo.trim() && this.todos.push(this.newTodo.trim());
      }
      this.newTodo = "";
    },
    removeTodo: function (idx) {
      this.todos.splice(idx, 1);
    },
  },
  components: {
    TodoItem,
    InputCustom,
  },
  watch: {
    sortBy: function () {
      this.todos.reverse();
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: white;
  margin: 0 0;
  padding: 0 5rem;
  background-color: rgb(36, 50, 63);
  display: flex;
  align-items: center;
  justify-content: center;
}
.main {
  width: 100%;
}
.todosList {
  margin: 1.5rem 0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  transition: all ease-in-out 150ms;
}

.todosList button:hover {
  background-color: rgb(47, 77, 104);
  color: white;
}
.shift {
  transform: translateX(-100%);
  opacity: 0;
  transition: all ease-in-out 100ms;
}
.resetHeight {
  height: 0;
  transition: all ease-in-out 150ms;
}
</style>
