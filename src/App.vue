<template>
  <button @click="add">Add</button>
  <div class="board">
    <div
      class="column-container"
      @drop="onDrop({ $event, list: todoColumnId })"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">TO DO</h2>
      <div
        v-for="item in getList(1)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag({ $event, item })"
      >
        <div class="task">{{ item.name }}</div>
      </div>
    </div>
    <div
      class="column-container"
      @drop="onDrop({ $event, list: inProgressColumnId })"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">IN PROGRESS</h2>
      <div
        v-for="item in getList(2)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag({ $event, item })"
      >
        <div class="task">{{ item.name }}</div>
      </div>
    </div>
    <div
      class="column-container"
      @drop="onDrop({ $event, list: doneColumnId })"
      @dragenter.prevent
      @dragover.prevent
    >
      <h2 class="title">DONE</h2>
      <div
        v-for="item in getList(3)"
        :key="item.id"
        class="task-list"
        draggable="true"
        @dragstart="startDrag({ $event, item })"
      >
        <div class="task">{{ item.name }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { createStore, createEvent } from "effector";
import { useVModel } from "effector-vue/composition";
import "./styles.css";
const todoColumnId = 1;
const inProgressColumnId = 2;
const doneColumnId = 3;

const add = createEvent("add");
const startDrag = createEvent();
const onDrop = createEvent();

const $todo = createStore([
  { name: "Veni", id: 0, list: todoColumnId },
  { name: "Vidi", id: 1, list: inProgressColumnId },
  { name: "Vici", id: 2, list: doneColumnId },
])
  .on(add, (state) => [...state, { name: "Puck", id: 4, list: todoColumnId }])
  .on(startDrag, (state, payload) => {
    payload.$event.dataTransfer.dropEffect = "move";
    payload.$event.dataTransfer.effectAllowed = "move";
    payload.$event.dataTransfer.setData("itemID", payload.item.id);
  })
  .on(onDrop, (state, payload) => {
    const oldState = state;
    const itemID = payload.$event.dataTransfer.getData("itemID");
    const item = oldState.filter((item) => item.id == itemID);
    item[0].list = payload.list;
    return [...oldState];
  });

const todo = useVModel($todo);

const getList = (list) => {
  return todo.value.filter((item) => item.list === list);
};
</script>
