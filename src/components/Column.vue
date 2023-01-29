<template>
  <div class="column-container">
    <h2 class="title">{{ title }}</h2>
    <draggable
      :list="tasks"
      :disabled="false"
      item-key="name"
      class="list-group"
      ghost-class="ghost"
      @start="dragging = true"
      @end="dragging = false"
    >
      <template class="task-list" #item="{ element }">
        <div class="task">{{ element.name }}</div>
      </template>
    </draggable>
  </div>
</template>

<style scoped></style>

<script setup>
import "./styles.css";
import draggable from "vuedraggable";

defineProps({
  title: {
    type: String,
    default: "",
  },
  tasks: {
    default: [],
  },
});

// const add = () => {
//   this.list.push({ name: "Juan " + id, id: id++ });
// };
// const replace = () => {
//   this.list = [{ name: "Edgard", id: id++ }];
// };
</script>
