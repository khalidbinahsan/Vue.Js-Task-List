<script setup>
import { ref, reactive } from "vue";
import Modal from "./components/Modal.vue";
import DeleteTask from "./components/DeleteModal.vue";
const deleteShow = ref(false);
const modalShow = ref(false);
const taskIndex = ref();
const childModalFunc = ref();
const modalClose = ()=>{
  modalShow.value = false;
}
const tasks = ref([
  { name: "Task 1", time: 60 },
  { name: "Task 2", time: 75 },
])

function removeTask(){
  tasks.value.splice(taskIndex.value, 1);
  deleteShow.value = false;
}
function dataPush(taskData){
  tasks.value.push(taskData);
  modalShow.value = false;
}
const callChildFunc = (dataIndex) => {
  taskIndex.value = dataIndex;
  childModalFunc.value.changeFormData(dataIndex);
  modalShow.value = true;
}
function childFuncModalOpn(){
  if(childModalFunc.value){
    childModalFunc.value.addNewTaskTitle();
  }
  modalShow.value = true;
}
function taskDelete(dataIndex){
  taskIndex.value = dataIndex;
  deleteShow.value = true;
}
function updateTask(updateData){
  tasks.value[taskIndex.value].name = updateData.name;
  tasks.value[taskIndex.value].time = updateData.time;
  modalShow.value = false;
}
</script>
<template>
  <Modal v-show="modalShow" @modal-hide="modalClose" @get-data="dataPush" :all-tasks="tasks" ref="childModalFunc" :task-index="taskIndex" @update-task="updateTask" />
  <DeleteTask v-show="deleteShow" @modal-hide="deleteShow = false" @delete-item="removeTask" />
  <section class="p-10 bg-green-200 m-auto rounded-md sm:w-4/5 md:w-3/5 lg:w-2/5">
    <div class="flex flex-col">
      <h1 class="text-3xl font-semibold">Vue.js Task List</h1>
      <h2 class="mt-2 text-lg font-semibold">Assignment 1</h2>
      <div class="m-auto mt-8 flex flex-col items-start w-full">
        <!-- Use the Modal here -->
        <button @click="childFuncModalOpn" class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-700">Add New Task</button>
        <div class="flex flex-col mt-2 w-full gap-2">
          <div v-show="tasks.length>0" v-for="(task, index) in tasks" :key="index" class="flex justify-between bg-slate-50 p-3 rounded-md w-full">
            <h3 class="font-medium"><span>{{ index + 1 }}. </span>{{ task.name }}</h3>
            <span class="font-medium">{{ task.time }} Min</span>
            <div>
              <span class="ml-2 text-blue-500 cursor-pointer" @click="callChildFunc(index)"><i class="fa-solid fa-pen-to-square"></i></span>
            <span class="ml-2 text-red-500 cursor-pointer" @click="taskDelete(index)"><i class="fa-solid fa-trash"></i></span>
          </div>
          </div>
          <h3 v-show="tasks.length<=0" class="text-start mt-2">No task today. Add a new task</h3>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
</style>
