<script setup>
import { onMounted, ref } from "vue";

const name = ref("Hello Vue!");
const status = ref("active");
const tasks = ref(["Task One", "Task Two", "Task Three"]);
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
    console.log(data);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <h1>
    {{ name }}
  </h1>
  <p v-if="status === 'active'">Status is true</p>
  <p v-else-if="status === 'pending'">Status is pending</p>
  <p v-else>Status is false</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Add</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <!-- <a v-bind:href="link">Google</a> -->
  <!-- <button v-on:click="toggleStatus">Activate</button> -->
  <button @click="toggleStatus">Activate</button>
</template>
<style scoped>
h1 {
  color: red;
}
</style>
