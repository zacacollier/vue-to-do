<template>
  <div>
    <button
      v-if="!showAddToDo"
      @click="toggleAddToDo"
    >
      Add To Do
    </button>
    <form
      @submit.prevent="handleSubmit"
      v-if="showAddToDo"
    >
      <input
        type="text"
        v-model="description"
        placeholder="Description"
      />
      <button type="submit">
        Add
      </button>
      <button @click.prevent="toggleAddToDo">
        Cancel
      </button>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ToDo } from './ToDoItem.vue';

const initialState = {
  description: '',
  showAddToDo: false,
};

export default Vue.extend({
  name: 'AddToDo',
  data() {
    return { ...initialState };
  },
  methods: {
    handleSubmit() {
      const { description } = this;
      this.$emit(
        'add-todo',
        new ToDo({
          description,
        }),
      );
      this.reset();
    },
    reset() {
      this.description = initialState.description;
    },
    toggleAddToDo() {
      this.showAddToDo = !this.showAddToDo;
    },
  },
});
</script>
