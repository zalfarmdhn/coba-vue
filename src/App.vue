<script setup>
  import { onMounted, ref } from 'vue';

  const name = ref('John Doe');
  const status = ref(true);
  const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
  const count = ref(0);
  const newTask = ref('');
  const checkTask = ref('');

  const toggleStatus = () => {
    status.value = !status.value;
  };

  const increaseCount = () => {
    count.value <= 10 && count.value++;
  };

  const decreaseCount = () => {
    count.value > 0 && count.value--;
  }

  const resetCount = () => {
    count.value = 0;
  }

  const addTask = () => {
    if (newTask.value !== '') {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  }

  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
  }

  onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title);
    } catch (error) {
      console.log('Error fetching data', error);
    }
  });
</script>

<template>
  <div class="box">
    <h1>{{ name }}</h1>
    <p v-if="status">User is active</p>
    <p v-else>User is inactive</p>

    <form @submit.prevent="addTask">
      <label for="newTask">Add Task</label>
      <input type="text" id="newTask" name="newTask" v-model="newTask">
      <button type="submit">Submit</button>
    </form>

    <h1>{{ checkTask }}</h1>
    
    <h3>Tasks</h3>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
        <span>
          {{ task }}
          <button @click="deleteTask(index)">X</button>
        </span>
      </li>
    </ul>
    <!-- <a v-bind:href="link">Click for google</a> -->
    <!-- <a :href="link">Click for google</a> -->

    <div class="box-2">
      <button @click="decreaseCount">Decrease</button>
      <p>{{ count }}</p>
      <button @click="increaseCount">Increase</button>
    </div>
    <button @click="resetCount">Reset Count</button>
    
  
    <button @click="toggleStatus">Click me!</button>
  </div>
</template>

<style scoped>
  .box {
    display: flex;
    flex-direction: column;
    gap: 0.93rem;
  }
  .box-2 {
    display: flex;
    flex-direction: row;
    gap: 0.93rem;
  }
</style>