<script setup>
import { ref, reactive } from "vue";
import Modal from "./components/Modal.vue";
import DeleteTask from "./components/DeleteModal.vue";
const deleteShow = ref(false);
const modalShow = ref(false);
const taskIndex = ref();
const tasks = ref([
  { name: "Task 1", time: 60 },
  { name: "Task 2", time: 75 },
])
function removeTask(){
  tasks.value.splice(taskIndex.value, 1);
  deleteShow.value = false;
}
function dataPush(taskData){
  console.log(taskData);
  tasks.value.push({
    name: taskData.value.name,
    time: taskData.value.time,
  });
  modalShow.value = false;
}
function taskDelete(dataIndex){
  taskIndex.value = dataIndex;
  deleteShow.value = true;
}
</script>
<template>
  <Modal v-show="modalShow" @modal-show="modalShow = false" @get-data="dataPush" />
  <DeleteTask v-show="deleteShow" @modal-hide="deleteShow = false" @delete-item="removeTask" />
  <section class="p-10 bg-green-200 w-3/5 m-auto rounded-md">
    <div class="flex flex-col">
      <h1 class="text-3xl font-semibold">Vue.js Task List</h1>
      <h2 class="mt-2 text-lg font-semibold">Assignment 1</h2>
      <div class="w-96 m-auto mt-8 flex flex-col items-start">
        <!-- Use the Modal here -->
        <button @click="modalShow = true" class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-700">Add New Task</button>
        <div class="flex flex-col mt-2 w-full gap-2">
          <div v-show="tasks.length>0" v-for="(task, index) in tasks" :key="index" class="flex justify-between bg-slate-50 p-3 rounded-md w-full">
            <h3 class="font-medium"><span>{{ index + 1 }}. </span>{{ task.name }}</h3>
            <span class="font-medium">{{ task.time }} Min</span>
            <span class="ml-2 text-red-500 cursor-pointer" @click="taskDelete(index)"><i class="fa-solid fa-trash"></i></span>
          </div>
          <h3 v-show="tasks.length<=0" class="text-start mt-2">No task today. Add a new task</h3>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
</style>
