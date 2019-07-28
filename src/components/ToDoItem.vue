<template>
  <div :id="todo.id">
    <input
      type="checkbox"
      v-model="todo.done"
    >
    <label
      for="item"
      :class="{
      done: todo.done
    }"
    >
      {{todo.description}}
    </label>
    <button
      v-show="!isEditing"
      @click="showEdit"
    >
      edit
    </button>
    <form
      v-show="isEditing"
      @submit.prevent="handleSubmit"
    >
      <input
        type="text"
        v-model="todo.description"
      />
      <button @click.prevent="hideEdit">
        (cancel)
      </button>
      <button type="submit">
        (update)
      </button>
    </form>
    <button @click.prevent="handleDelete(todo)">
      x
    </button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export class ToDo {
  id: number;

  done: boolean;

  description: string;

  constructor({
    description,
    done = false,
    id = Date.now(),
  }: {
    id?: number;

    done?: boolean;

    description: string;
  }) {
    this.description = description;
    this.done = done;
    this.id = id;
  }
}

export default Vue.extend({
  name: 'ToDoItem',
  props: {
    todo: Object,
  },
  data() {
    return {
      isEditing: false,
    };
  },
  methods: {
    handleDelete(todo: ToDo) {
      this.$emit('delete-todo', todo);
      this.hideEdit();
    },
    showEdit() {
      this.isEditing = true;
    },
    hideEdit() {
      this.isEditing = false;
    },
    handleSubmit() {
      this.$emit('update-todo', this.todo.id, this.todo.description);
      this.hideEdit();
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
.done {
  text-decoration: line-through;
  font-style: italic;
}
</style>
