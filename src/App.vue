<template>
  <div class="board">
    <button @click="add">Add</button>
    <button @click="remove">Remove</button>
    <button @click="reset">reset</button>

    <Column title="To do" :tasks="todo"></Column>
    <Column title="In Progress" :tasks="inProgress"></Column>
    <Column title="Testing"></Column>
    <Column title="Done"></Column>
  </div>
</template>

<script setup>
import { reactive } from "vue";

import { createStore, createEvent } from "effector";
import { useVModel } from "effector-vue/composition";
import Column from "./components/Column.vue";
import "./styles.css";

const inProgress = reactive([
  { name: "John", id: 0 },
  { name: "Joao", id: 1 },
  { name: "Jean", id: 2 },
]);

const add = createEvent("add");
const remove = createEvent("remove");
const reset = createEvent("reset");

const $todo = createStore([
  { name: "John", id: 0 },
  { name: "Joao", id: 1 },
  { name: "Jean", id: 2 },
])
  .on(add, (state) => [...state, { name: "Puck", id: 4 }])
  .on(remove, (state) => {
    state.length = state.length - 1;
    return [...state];
  })
  .reset(reset);

const todo = useVModel($todo);
</script>
