<template>
  <div>
    <ToDoItem
      v-for="todo in filteredToDos"
      :todo.sync="todo"
      :key="`item-${todo.id}`"
      @delete-todo="deleteToDo"
      @update-todo="updateToDo"
    />
    <AddToDo @add-todo="addToDo" />
    <button
      @click="showAll"
      :disabled="visibility === 'all'"
    >
      Show All
    </button>
    <button
      @click="onlyShowComplete"
      :disabled="visibility === 'complete'"
    >
      Hide Incomplete
    </button>
    <button
      @click="onlyShowIncomplete"
      :disabled="visibility === 'incomplete'"
    >
      Hide Done
    </button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import ToDoItem, { ToDo } from './ToDoItem.vue';
import AddToDo from './AddToDo.vue';

enum Visibility {
  all = 'all',
  complete = 'complete',
  incomplete = 'incomplete'
}

const visibilityFilters = {
  all(todos: ToDo[]) {
    return todos;
  },
  complete(todos: ToDo[]) {
    return todos.filter(todo => todo.done);
  },
  incomplete(todos: ToDo[]) {
    return todos.filter(todo => !todo.done);
  },
};

export default Vue.extend({
  name: 'ToDoList',
  components: {
    AddToDo,
    ToDoItem,
  },
  data(): { todos: ToDo[]; visibility: Visibility } {
    return {
      todos: [],
      visibility: Visibility.all,
    };
  },
  methods: {
    showAll() {
      this.visibility = Visibility.all;
    },
    onlyShowIncomplete() {
      this.visibility = Visibility.incomplete;
    },
    onlyShowComplete() {
      this.visibility = Visibility.complete;
    },
    addToDo(todo: ToDo) {
      this.todos = [...this.todos, todo];
    },
    deleteToDo(todo: ToDo) {
      const targetIndex = this.todos.findIndex(({ id }) => id === todo.id);
      this.todos.splice(targetIndex, 1);
    },
    updateToDo(id: number, newDescription: string) {
      const targetIndex = this.todos.findIndex(todo => todo.id === id);
      this.todos[targetIndex].description = newDescription;
    },
  },
  computed: {
    filteredToDos(): ToDo[] {
      return visibilityFilters[this.visibility](this.todos);
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
