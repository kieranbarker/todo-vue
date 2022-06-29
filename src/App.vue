<script>
import Form from "./components/Form.vue";
import ToDoList from "./components/ToDoList.vue";
import { getStorage, setStorage } from "./storage";

export default {
  components: {
    Form,
    ToDoList,
  },
  data() {
    return {
      toDos: getStorage(),
      editing: null,
    };
  },
  methods: {
    addToDo(value) {
      const toDo = { name: value, done: false };
      this.toDos = [...this.toDos, toDo];
    },
    toggleToDo(index) {
      this.toDos = this.toDos.map((toDo, i) => {
        if (i !== index) return toDo;
        return { ...toDo, done: !toDo.done };
      });
    },
    setEditing(index) {
      this.editing = index;
    },
    editToDo(value) {
      this.toDos = this.toDos.map((toDo, index) => {
        if (index !== this.editing) return toDo;
        return { ...toDo, name: value };
      });
      this.setEditing(null);
    },
    deleteToDo(index) {
      this.editing = null;
      this.toDos = this.toDos.filter((toDo, i) => i !== index);
    },
  },
  watch: {
    toDos(newToDos) {
      setStorage(newToDos);
    },
  },
};
</script>

<template>
  <Form
    :editing="editing"
    :to-dos="toDos"
    @edit-todo="editToDo"
    @add-todo="addToDo"
  ></Form>

  <p v-if="toDos.length < 1">
    <em>Add some to-dos...</em>
  </p>

  <ToDoList
    v-else
    :to-dos="toDos"
    @toggle-todo="toggleToDo"
    @set-editing="setEditing"
    @delete-todo="deleteToDo"
  />
</template>
