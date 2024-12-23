<script  setup>

import { ref  } from "vue";

// Define prop and emit  variables
const newTask = ref(""); // Holds the input for new task
const tasks = ref([]); // Array to store tasks

const emit = defineEmits(["addTask"]);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    emit("addTask", {
      id: Date.now(),  // Unique ID
      text: newTask.value.trim(),
      completed: false,
    });
    newTask.value = ""; // Clear input after adding the task
  }
};

</script>
<template>
<div class="flex items-center mb-4">
          <input
            type="text"
            class="flex-1 border rounded-1 py-2 px-3 focus:outline-none text-gray-800"
            placeholder="Add a new task... "
            v-model="newTask"
          />
          <button
            class="bg-blue-400 hover:bg-red-500 text-white px-4 py-2 rounded-r focus:outline-none"
            @click="addTask"
          >
            <i class="fa-solid fa-plus"></i>
          </button>
        </div>
</template>