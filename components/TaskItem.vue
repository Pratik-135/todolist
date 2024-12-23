<!-- TaskItem.vue -->
<template>
    <div>
      <input
        type="checkbox"
        v-model="task.completed"
        @change="toggleTaskStatus"
        class="mr-2 checkbox h-3 w-4 text-blue-500"
      />
      <span :class="{'line-through text-gray-500': task.completed }">
        {{ task.text }}
      </span>
      
      <button @click="editTask" class="text-blue-400 hover:text-red-600 focus:outline-none pr-1 pl-2">
        <i class="fa fa-pencil-square"></i>
      </button>
      <button @click="removeTask" class="text-red-400 hover:text-red-600 focus:outline-none pl-1">
        <i class="fa-solid fa-trash"></i>
      </button>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    task: Object,
    index: Number
  });
  
  const emit = defineEmits(["remove", "toggle", "edit"]);
  
  const toggleTaskStatus = () => {
    emit("toggle", props.index);
  };
  
  const removeTask = () => {
    emit("remove", props.index);
  };
  
  const editTask = () => {
    const editedText = prompt("Edit task:", props.task.text);
    if (editedText) {
      emit("edit", { index: props.index, text: editedText });
    }
  };
  </script>
  