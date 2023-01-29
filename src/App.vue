<template>
  <div class="board">
    <Column title="To do" :tasks="todo"></Column>
    <Column title="In Progress" :tasks="inProgress"></Column>
    <Column title="Testing"></Column>
    <Column title="Done"></Column>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import Column from "./components/Column.vue";
import "./styles.css";

const todo = reactive([
  { name: "John", id: 0 },
  { name: "Joao", id: 1 },
  { name: "Jean", id: 2 },
]);

const inProgress = reactive([
  { name: "John", id: 0 },
  { name: "Joao", id: 1 },
  { name: "Jean", id: 2 },
]);
</script>
