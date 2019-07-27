<template>
  <div>
    <input
      id="item"
      type="checkbox"
      v-model="done"
    >
    <label
      for="item"
      :class="{
      done
    }"
    >
      {{description}}
    </label>
    <button
      v-if="!showEditToDo"
      @click="toggleEditing"
    >
      edit
    </button>
    <form
      v-if="showEditToDo"
      @submit.prevent="handleSubmit"
    >
      <input
        type="text"
        v-model="newDescription"
      />
      <button @click.prevent="toggleEditing">
        (cancel)
      </button>
      <button type="submit">
        (update)
      </button>
    </form>
    <button @click="handleRemove">
      x
    </button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

const initialState = {
  done: false,
  showEditToDo: false,
  newDescription: '',
};

export default Vue.extend({
  name: 'ToDoItem',
  props: {
    removeToDo: Function,
    updateToDo: Function,
    description: String,
    id: Number,
  },
  data() {
    return initialState;
  },
  methods: {
    handleRemove() {
      return this.removeToDo(this.id);
    },
    toggleEditing() {
      this.showEditToDo = !this.showEditToDo;
      this.resetNewDescription();
    },
    resetNewDescription() {
      if (this.newDescription === initialState.newDescription) {
        this.newDescription = this.description;
      } else {
        this.newDescription = initialState.newDescription;
      }
    },
    handleSubmit() {
      this.updateToDo(this.id, this.newDescription);
      this.toggleEditing();
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
