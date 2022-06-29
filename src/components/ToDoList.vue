<script>
export default {
  emits: ["toggle-todo", "set-editing", "delete-todo"],

  props: {
    toDos: {
      type: Array,
      required: true,
    },
  },

  methods: {
    deleteToDo(name, index) {
      const message = `Are you sure you want to delete '${name}'?`;

      const confirmDelete = window.confirm(message);
      if (!confirmDelete) return;

      this.$emit("delete-todo", index);
    },
  },
};
</script>

<template>
  <ul role="list">
    <li v-for="(toDo, index) in toDos" :key="index">
      <input
        type="checkbox"
        :id="`toDo-${index}`"
        :checked="toDo.done"
        @change="$emit('toggle-todo', index)"
      />

      <label :for="`toDo-${index}`">{{ toDo.name }}</label>

      <button
        type="button"
        data-action="edit"
        :aria-label="`Edit '${toDo.name}'`"
        @click="$emit('set-editing', index)"
      >
        Edit
      </button>

      <button
        type="button"
        data-action="delete"
        :aria-label="`Delete '${toDo.name}'`"
        @click="deleteToDo(toDo.name, index)"
      >
        Delete
      </button>
    </li>
  </ul>
</template>

<style scoped>
ul {
  margin-top: 2em;
}

li {
  display: flex;
  gap: 0.5em;
  align-items: center;
}

li + li {
  margin-top: 1em;
}

[type="checkbox"] {
  margin: 0 0.25em 0 0;
}

:checked + label {
  text-decoration: line-through;
}

[data-action] {
  padding: 0 0.25em;
  border: 0;
  border-radius: 0.25em;
  margin: 0;
  background: 0;
  cursor: pointer;
  font-size: smaller;
  text-transform: lowercase;
}

[data-action="edit"] {
  margin-left: auto;
  color: black;
  background: #ffc107;
}

[data-action="delete"] {
  color: white;
  background: #dc3545;
}
</style>
