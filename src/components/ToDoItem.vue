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
    <button @click="handleRemove">
      x
    </button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'ToDoItem',
  props: {
    removeToDo: Function,
    description: String,
    id: Number,
  },
  data() {
    return {
      done: false,
    };
  },
  methods: {
    handleRemove() {
      return this.removeToDo(this.id);
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
