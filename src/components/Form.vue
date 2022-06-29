<script>
export default {
  emits: ["edit-todo", "add-todo"],
  props: {
    editing: {
      type: [Number, null],
      required: true,
    },
    toDos: {
      type: Array,
      required: true,
    },
  },
  computed: {
    inputValue() {
      return this.toDos[this.editing]?.name ?? "";
    },
    buttonText() {
      return this.toDos[this.editing] ? "Save to-do" : "Add to-do";
    },
  },
  methods: {
    onSubmit(event) {
      const input = event.target.elements["to-do"];
      if (!input) return;

      const value = input.value.trim();
      if (!value) return;

      input.value = "";

      if (this.toDos[this.editing]) {
        this.$emit("edit-todo", value);
      } else {
        this.$emit("add-todo", value);
      }
    },
  },
  updated() {
    if (this.toDos[this.editing]) {
      this.$refs.input.focus();
    }
  },
};
</script>

<template>
  <form @submit.prevent="onSubmit">
    <p>
      <label for="to-do">What do you need to do?</label>
      <input id="to-do" type="text" ref="input" :value="inputValue" required />
    </p>
    <p>
      <button type="submit">{{ buttonText }}</button>
    </p>
  </form>
</template>

<style scoped>
label,
input {
  display: block;
  width: 100%;
}
</style>
