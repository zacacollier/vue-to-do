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
        v-model="todo.description"
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

const initialState = {
  todo: {
    description: '',
  },
  showAddToDo: false,
};

export default Vue.extend({
  name: 'AddToDo',
  props: {
    addToDo: Function,
  },
  data() {
    return initialState;
  },
  methods: {
    handleSubmit() {
      const newToDo = { ...this.todo, id: Date.now() };
      this.addToDo(newToDo);
      this.resetToDo();
    },
    resetToDo() {
      this.todo = { ...initialState.todo };
    },
    toggleAddToDo() {
      this.showAddToDo = !this.showAddToDo;
    },
  },
});
</script>
