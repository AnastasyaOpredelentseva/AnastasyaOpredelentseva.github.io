<template>
  <div class="container">
    <button class="button" @click="() => TogglePopup('addTrigger')">
      Add new task
    </button>
    <Popup
      v-if="popupTriggers.addTrigger"
      :TogglePopup="() => TogglePopup('addTrigger')"
    >
      <h2>Add new Element</h2>
      <input
        class="input"
        type="text"
        placeholder="Enter task name"
        v-model="newTaskNameInput"
      />
      <button class="button" @click="add">Add new task!</button>
    </Popup>

    <Popup
      v-if="popupTriggers.editTrigger"
      :TogglePopup="() => TogglePopup('editTrigger')"
    >
      <h2>Edit task</h2>
      <input
        class="input"
        type="text"
        placeholder="Change task name"
        v-model="changedTaskNameInput"
      />
      <button class="button" @click="change">Change task!</button>
      <button class="button" @click="remove">Remove task!</button>
    </Popup>
    <div class="board">
      <ColumnContainer
        :onDrop="($event) => onDrop({ $event, list: todoColumnId })"
        :startDrag="($event, item) => startDrag({ $event, item })"
        :title="`TO DO`"
        :list="getList(todoColumnId)"
        :handleEditTask="(item) => handleEditTask(item)"
      />
      <ColumnContainer
        :onDrop="($event) => onDrop({ $event, list: inProgressColumnId })"
        :startDrag="($event, item) => startDrag({ $event, item })"
        :title="`IN PROGRESS`"
        :list="getList(inProgressColumnId)"
        :handleEditTask="(item) => handleEditTask(item)"
      />
      <ColumnContainer
        :onDrop="($event) => onDrop({ $event, list: doneColumnId })"
        :startDrag="($event, item) => startDrag({ $event, item })"
        :title="`DONE`"
        :list="getList(doneColumnId)"
        :handleEditTask="(item) => handleEditTask(item)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { createStore, createEvent } from "effector";
import Popup from "./components/Popup.vue";
import ColumnContainer from "./components/ColumnContainer.vue";
import { useVModel } from "effector-vue/composition";
import "./styles.css";
const todoColumnId = 1;
const inProgressColumnId = 2;
const doneColumnId = 3;
const newTaskNameInput = ref("");
const changedTaskNameInput = ref("");
const changedTaskId = ref("");

const add = createEvent();
const change = createEvent();
const remove = createEvent();
const startDrag = createEvent();
const onDrop = createEvent();

const $todo = createStore([
  { name: "Veni", id: 0, list: todoColumnId },
  { name: "Vidi", id: 1, list: inProgressColumnId },
  { name: "Vici", id: 2, list: doneColumnId },
])
  .on(add, (state) => {
    const newTaskName = newTaskNameInput.value;
    newTaskNameInput.value = "";
    TogglePopup("addTrigger");
    return [
      ...state,
      { name: newTaskName, id: state.length, list: todoColumnId },
    ];
  })
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
  })
  .on(change, (state) => {
    const updatedTaskName = changedTaskNameInput.value;
    const updatedTaskId = changedTaskId.value;
    changedTaskId.value = "";
    changedTaskNameInput.value = "";
    const oldState = state;
    const item = oldState.filter((item) => item.id == updatedTaskId);
    item[0].name = updatedTaskName;
    TogglePopup("editTrigger");
    return [...oldState];
  })
  .on(remove, (state) => {
    const removedTaskId = changedTaskId.value;
    changedTaskId.value = "";
    TogglePopup("editTrigger");
    return [...state.filter((item) => item.id !== removedTaskId)];
  });

const todo = useVModel($todo);

const getList = (list) => {
  return todo.value.filter((item) => item.list === list);
};

const popupTriggers = ref({
  addTrigger: false,
  editTrigger: false,
});

const TogglePopup = (trigger) => {
  popupTriggers.value[trigger] = !popupTriggers.value[trigger];
};

const handleEditTask = ({ name, id }) => {
  TogglePopup("editTrigger");
  changedTaskNameInput.value = name;
  changedTaskId.value = id;
};
</script>
