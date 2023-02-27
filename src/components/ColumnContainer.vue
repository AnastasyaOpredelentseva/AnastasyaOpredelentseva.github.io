<template>
  <div
    class="column-container"
    @drop="onDrop($event)"
    @dragenter.prevent
    @dragover.prevent
  >
    <h2 class="title">{{ title }}</h2>
    <div
      v-for="item in list"
      :key="item.id"
      class="task-list"
      draggable="true"
      @dragstart="startDrag($event, item)"
    >
      <div class="task" @click="handleEditTask(item)">
        {{ item.name }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["onDrop", "startDrag", "title", "list", "handleEditTask"],
};
</script>
