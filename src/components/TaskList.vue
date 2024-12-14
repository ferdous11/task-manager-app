<template>
    <div class="task-list">
      <h1>Task Management App</h1>
  
      <!-- Button to toggle form visibility -->
      <button @click="isFormVisible = !isFormVisible" class="toggle-form-btn">
        {{ isFormVisible ? 'Hide Add Task Form' : 'Show Add Task Form' }}
      </button>
  
      <!-- Form for adding a new task -->
      <form v-if="isFormVisible" @submit.prevent="addTask" class="task-form">
        <input
          v-model="newTask.name"
          type="text"
          placeholder="Enter task name"
          required
        />
        <button type="submit" class="add-task-btn">Add Task</button>
      </form>
  
      <!-- Task List -->
      <div>
        <ul>
          <li
            v-for="(task, index) in tasks"
            :key="index"
            :class="{ completed: task.completed }"
          >
            <!-- Task Name -->
            <span>{{ task.name }}</span>
  
            <!-- Buttons -->
            <div class="task-buttons">
              <button
                v-if="!task.completed"
                @click="markAsCompleted(task)"
                class="complete-btn"
              >
                Complete
              </button>
              <button
                v-else
                @click="undoTask(task)"
                class="undo-btn"
              >
                Undo
              </button>
              <button @click="deleteTask(index)" class="delete-btn">
                Delete
              </button>
            </div>
          </li>
        </ul>
        <p v-if="tasks.length === 0">No tasks available</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  // Reactive state for tasks
  const tasks = ref([]);
  
  // New task form state
  const newTask = ref({
    name: '',
    completed: false,
  });
  
  // Form visibility toggle
  const isFormVisible = ref(true);
  
  // Add a new task
  const addTask = () => {
    if (newTask.value.name.trim().length >= 3) {
      tasks.value.push({ ...newTask.value });
      saveTasks();
      newTask.value.name = '';
    } else {
      alert('Task name must be at least 3 characters long.');
    }
  };
  
  // Mark a task as completed
  const markAsCompleted = (task) => {
    task.completed = true;
    saveTasks();
  };
  
  // Undo a completed task
  const undoTask = (task) => {
    task.completed = false;
    saveTasks();
  };
  
  // Delete a task
  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
    saveTasks();
  };
  
  // Persist tasks to localStorage
  const saveTasks = () => {
    localStorage.setItem('tasks', JSON.stringify(tasks.value));
  };
  
  // Load tasks from localStorage on component mount
  onMounted(() => {
    const savedTasks = localStorage.getItem('tasks');
    if (savedTasks) tasks.value = JSON.parse(savedTasks);
  });
  </script>
  
  <style scoped>
  .task-list {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  h1 {
    text-align: center;
  }
  
  .task-form {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 20px;
  }
  
  .task-form input {
    flex: 1;
    padding: 8px;
    font-size: 16px;
  }
  
  .add-task-btn {
    background-color: purple;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
  }
  
  .toggle-form-btn {
    display: block;
    margin: 0 auto 20px;
    background-color: purple;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    margin-bottom: 10px;
    background-color: #f9f9f9;
    border-radius: 5px;
  }
  
  li.completed {
    background-color: lightgreen;
    border: 1px solid red;
  }
  
  .task-buttons {
    display: flex;
    gap: 10px;
  }
  
  .complete-btn,
  .undo-btn,
  .delete-btn {
    background-color: purple;
    color: white;
    border: none;
    padding: 6px 12px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 14px;
  }
  
  .delete-btn {
    background-color: red;
  }
  
  .complete-btn:hover,
  .undo-btn:hover,
  .delete-btn:hover {
    opacity: 0.8;
  }
  </style>
  