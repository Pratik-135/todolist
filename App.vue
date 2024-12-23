<script setup>
import { ref } from 'vue';

// Define reactive variables
const newTask = ref('');
const tasks = ref([]);

// Method to add a task
const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      text: newTask.value.trim(),
      completed: false,
      date: new Date().toLocaleDateString(),
      editing: false,
    });
    newTask.value = '';
  }
};

// Method to remove a task
const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

// Method to toggle task completion
const toggleTaskStatus = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
};

// Method to clear completed tasks
const clearCompleteTasks = () => {
  tasks.value = tasks.value.filter((task) => !task.completed);
};

// Method to edit a task
const editTask = (index) => {
  tasks.value[index].editing = true;
};

// Method to save an edited task
const saveTask = (index, newText) => {
  if (newText.trim() !== '') {
    tasks.value[index].text = newText.trim();
  }
  tasks.value[index].editing = false;
};
</script>

<template>
  <div class="bg-gradient-to-br from-gray-200 flex justify-center items-center h-screen text-gray-600">
    <div class="bg-opacity-50 backdrop-filter backdrop-blur-lg rounded-lg shadow-lg p-8 max-w-md w-full">
      <h1 class="text-black text-4xl font-bold text-center mb-8">ToDo List</h1>
      <div class="max-w-md">
        <div class="flex items-center mb-4">
          <input
            type="text"
            class="flex-1 border rounded-lg py-2 px-3 focus:outline-none text-gray-800"
            placeholder="Add a new task..."
            v-model="newTask"
            @keyup.enter="addTask"
          />
          <button
            class="bg-blue-400 hover:bg-red-500 text-white px-4 py-2 rounded-r focus:outline-none"
            @click="addTask"
          >
            <i class="fa-solid fa-plus"></i>
          </button>
        </div>
        <ul>
          <li
            v-for="(task, index) in tasks"
            :key="index"
            class="flex items-center justify-between border-b py-4"
          >
            <div v-if="!task.editing" class="flex items-center">
              <input
                type="checkbox"
                class="mr-2 form-checkbox h-4 w-4 text-blue-500"
                v-model="task.completed"
              />
              <span
                :class="{ 'line-through': task.completed }"
                class="text-gray-900"
              >
                {{ task.text }}
              </span>
              <span class="text-sm text-gray-600 ml-2">{{ task.date }}</span>
            </div>
            <div v-else>
              <input
                type="text"
                class="border rounded py-2 px-3 text-gray-800 w-full"
                :value="task.text"
                @keyup.enter="saveTask(index, $event.target.value)"
              />
            </div>
            <div>
              <button
                @click="task.editing ? saveTask(index, task.text) : editTask(index)"
                class="text-blue-400 hover:text-blue-600 focus:outline-none pr-2"
              >
                <i class="fa fa-pencil-square" aria-hidden="true"></i>
              </button>
              <button
                @click="removeTask(index)"
                class="text-red-400 hover:text-red-600 focus:outline-none pl-2"
              >
                <i class="fa-solid fa-trash"></i>
              </button>
            </div>
          </li>
        </ul>
        <button
          @click="clearCompleteTasks"
          class="mt-8 border bg-red-400 hover:bg-red-600 px-6 py-2 rounded-lg focus:outline-none text-white w-full gap-2"
        >
          Clear Completed
        </button>
      </div>
    </div>
  </div>
</template>
