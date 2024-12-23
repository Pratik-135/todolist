<template>
    <div>
        <!-- TaskInput component to add tasks -->
    <TaskInput @addTask="addTask" />
      <ul>
        <TaskItem
          v-for="(task, index) in tasks"
          :key="task.id"  
          :task="task"
          :index="index"
          @remove="removeTask"
          @toggle="toggleTaskStatus"
          @edit="editTask"
        />
      </ul>
      <button  class="rounded-lg bg-red-600 p-4 text-white" @click="clearCompleteTasks">Clear Completed</button>
    
    </div>

  </template>
  
  <script setup>
  import { ref, watch, onMounted } from "vue";
  import TaskItem from "./TaskItem.vue";
  import TaskInput from "./TaskInput.vue";
  
  const tasks = ref([]);

  // Method to add a new task
const addTask = (newTask) => {
  tasks.value.push(newTask);
};
  
  onMounted(() => {
    const storedTasks = localStorage.getItem("tasks");
    if (storedTasks) {
      tasks.value = JSON.parse(storedTasks);
    }
  });
  
  watch(
    tasks,
    (newTasks) => {
      localStorage.setItem("tasks", JSON.stringify(newTasks));
    },
    { deep: true }
  );
  
  const removeTask = (index) => {
    tasks.value.splice(index, 1);
  };
  
  const toggleTaskStatus = (index) => {
    tasks.value[index].completed = !tasks.value[index].completed;
  };
  
  const editTask = ({ index, text }) => {
    tasks.value[index].text = text;
  };
  
  const clearCompleteTasks = () => {
    tasks.value = tasks.value.filter((task) => !task.completed);
  };
  </script>
  
