<script setup>
import { ref } from "vue";
import { Icon } from "@iconify/vue";

const showOverlay = ref(false);
const input = ref("");
const todos = ref([]);
const errorMessage = ref("");
const darkMode = ref(false);

const getRandomColor = () => {
  const hue = Math.floor(Math.random() * 360);
  const saturation = "100%";
  const lightness = "75%";
  return `hsl(${hue}, ${saturation}, ${lightness})`;
};

const addTodo = () => {
  if (input.value.trim().length < 10) {
    errorMessage.value = "Please enter Minimum 10 Characters";
    return;
  }
  todos.value.push({
    id: Math.floor(Math.random() * 100000),
    text: input.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showOverlay.value = false;
  input.value = "";
  errorMessage.value = "";
};

const deleteTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
};
</script>

<template>
  <div :class="{ dark: darkMode }" class="w-full min-h-screen bg-slate-200">
    <!-- Overlay -->
    <div
      v-if="showOverlay"
      class="absolute w-full h-screen bg-[rgb(0,0,0,0.77)] flex justify-center items-center"
    >
      <div
        class="max-w-[1000px] w-[750px] bg-white rounded-[10px] p-8 relative flex flex-col"
      >
        <textarea
          v-model="input"
          :class="{ dark: darkMode }"
          class="border border-gray-400 focus:outline-none p-4"
          placeholder="Write Todo Here..."
          name="todo"
          id="todo"
          cols="30"
          rows="10"
        ></textarea>
        <!-- Error Message -->
        <p v-if="errorMessage" class="text-red-500 font-bold mt-3">
          {{ errorMessage }}
        </p>
        <div class="flex flex-col gap-4 mt-10">
          <button @click="addTodo" class="bg-blue-400 py-2">Add Todo</button>
          <button @click="showOverlay = false" class="bg-red-500 py-2">
            Close
          </button>
        </div>
      </div>
    </div>
    <!-- Header -->
    <header
      class="max-w-[1000px] flex flex-row items-center justify-between mx-auto pt-20 text-4xl"
    >
      <h1>Notes</h1>
      <div>
        <Icon
          @click="darkMode = false"
          v-show="darkMode"
          icon="openmoji:sun-with-face"
          width="30"
          class="cursor-pointer mt-3"
        />
        <Icon
          @click="darkMode = true"
          v-show="!darkMode"
          icon="openmoji:last-quarter-moon-face"
          width="30"
          class="cursor-pointer mt-3"
        />
      </div>
      <button
        @click="showOverlay = true"
        class="bg-black text-white items-center text-center rounded-full w-12 h-12"
      >
        +
      </button>
    </header>
    <!-- Notes Card -->
    <div class="flex flex-row flex-wrap max-w-[1000px] mt-28 mx-auto gap-12">
      <div
        v-for="todo in todos"
        :key="todo.id"
        :style="{ backgroundColor: todo.backgroundColor }"
        class="bg-yellow-300 w-[250px] h-[300px] overflow-auto rounded-lg flex flex-col justify-between p-4"
      >
        <h1 class="whitespace-normal break-words">
          {{ todo.text }}
        </h1>
        <div class="flex flex-row items-center justify-between">
          <p class="font-bold">{{ todo.date.toLocaleDateString("en-US") }}</p>
          <Icon
            @click="deleteTodo(todo.id)"
            icon="material-symbols-light:delete"
            width="25"
            class="cursor-pointer"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body {
  background-color: #f3f4f6;
}

.dark {
  background-color: #1f2937;
}

.dark {
  color: white;
}
</style>
