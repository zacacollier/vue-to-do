<template>
  <div>
    <ToDoItem
      v-for="({ id, description, done }, index) in todos"
      :key="`item-${index}`"
      :id="id"
      :description="description"
      :done="done"
      :removeToDo="removeToDo"
    />
    <AddToDo :addToDo="addToDo" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import ToDoItem from './ToDoItem.vue';
import AddToDo from './AddToDo.vue';

interface ToDo {
  id: number;
  done: boolean;
  description: string;
}

export default Vue.extend({
  name: 'ToDoList',
  components: {
    AddToDo,
    ToDoItem,
  },
  data(): { todos: ToDo[] } {
    return {
      todos: [],
    };
  },
  methods: {
    addToDo(todo: ToDo) {
      this.todos = [...this.todos, todo];
    },
    removeToDo(id: number) {
      this.todos = this.todos.filter(todo => todo.id !== id);
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
