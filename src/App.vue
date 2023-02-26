<template>
  <button @click="add">Add</button>

  <div class="board">
    <div
      class="column-container"
      @drop="onDrop($event, 1)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">TO DO</h2>
      <div
        v-for="item in getList(1)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag($event, item)"
      >
        <div class="task">{{ item.title }}</div>
      </div>
    </div>
    <div
      class="column-container"
      @drop="onDrop($event, 2)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">IN PROGRESS</h2>
      <div
        v-for="item in getList(2)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag($event, item)"
      >
        <div class="task">{{ item.title }}</div>
      </div>
    </div>
    <div
      class="column-container"
      @drop="onDrop($event, 3)"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">DONE</h2>
      <div
        v-for="item in getList(3)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag($event, item)"
      >
        <div class="task">{{ item.title }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import "./styles.css";

export default {
  setup() {
    const items = ref([
      { id: 0, title: "A", list: 1 },
      { id: 1, title: "B", list: 1 },
      { id: 2, title: "C", list: 1 },
      { id: 3, title: "D", list: 2 },
      { id: 3, title: "D", list: 3 },
    ]);

    const getList = (list) => {
      return items.value.filter((item) => item.list === list);
    };

    const startDrag = (event, item) => {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id);
    };
    const onDrop = (event, list) => {
      const itemID = event.dataTransfer.getData("itemID");
      const item = items.value.find((item) => item.id == itemID);
      item.list = list;
    };
    const add = () => {
      items.value.push({ id: items.value.length, title: "D", list: 1 });
    };
    return {
      getList,
      onDrop,
      add,
      startDrag,
    };
  },
};
</script>
