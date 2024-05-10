<template>
  <div class="container">
    <h1 class="title">To Do List</h1>
    <div class="add-task">
      <input type="text" v-model="newTask" placeholder="Add new task...">
      <input type="text" v-model="newCategory" placeholder="Category...">
      <button @click="addTask">Add</button>
    </div>

    <table class="task-table">
      <thead>
        <tr>
          <th></th>
          <th>Task</th>
          <th>Category</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="index" :class="{ 'completed': task.completed }">
          <td><input type="checkbox" v-model="task.completed" class="checkbox"></td>
          <td>
            <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.title" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!task.editing" class="task-text">{{ task.category }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.category" class="edit-input">
            </div>
          </td>
          <td>
            <div v-if="!task.editing" class="button-group">
              <button class="edit-btn" @click="editTask(index)">Edit</button>
              <button class="delete-btn" @click="deleteTask(index)">Delete</button>
            </div>
            <div v-else class="button-group">
              <button class="update-btn" @click="updateTask(index)">Update</button>
              <button class="cancel-btn" @click="cancelTask(index)">Cancel</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="filters">
      <select v-model="selectedCategory" @change="filterByCategory">
        <option value="">All Categories</option>
        <option v-for="category in categories" :key="category">{{ category }}</option>
      </select>
      <input type="text" v-model="searchQuery" placeholder="Search tasks...">
    </div>
    
    <button class="filter-btn" @click="toggleShowIncomplete">
      {{ showIncomplete ? 'Show All Tasks' : 'Show Incomplete Tasks Only' }}
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: '',
      newCategory: '',
      showIncomplete: true,
      selectedCategory: '',
      searchQuery: ''
    };
  },
  computed: {
    filteredTasks() {
      let filtered = this.tasks.filter(task => {
        if (this.showIncomplete && task.completed) return false;
        if (this.selectedCategory && task.category !== this.selectedCategory) return false;
        if (this.searchQuery && !task.title.toLowerCase().includes(this.searchQuery.toLowerCase())) return false;
        return true;
      });
      return filtered;
    },
    categories() {
      let categories = new Set(this.tasks.map(task => task.category));
      return Array.from(categories);
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ title: this.newTask, completed: false, category: this.newCategory || 'Uncategorized' });
        this.newTask = '';
        this.newCategory = '';
      }
    },
    cancelTask(index) {
      this.tasks[index].editing = false;
    },
    editTask(index) {
      this.tasks[index].editing = true;
    },
    updateTask(index) {
      this.tasks[index].editing = false;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleShowIncomplete() {
      this.showIncomplete = !this.showIncomplete;
    },
    filterByCategory() {
      // Implement if needed
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9; /* Warna latar belakang */
}

.title {
  text-align: center;
  color: #333;
}

.add-task {
  margin-bottom: 20px;
}

.add-task input[type="text"] {
  margin-right: 10px;
  padding: 8px;
  width: 200px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.add-task button {
  padding: 8px 15px;
  background-color: #4CAF50;
  color: #fff;
  border: 1px solid #4CAF50; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.add-task button:hover {
  background-color: #45a049;
}

.task-table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.task-table th,
.task-table td {
  border: 1px solid #ddd;
  padding: 10px;
}

.task-table th {
  background-color: #4CAF50;
  color: #fff;
  text-align: left;
}

.task-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

.checkbox {
  margin: 0;
}

.edit-input {
  width: 100%;
}

.button-group {
  display: flex;
  justify-content: flex-start;
  gap: 10px;
}

.button-group button {
  padding: 8px 15px;
  background-color: transparent;
  color: #4CAF50;
  border: 1px solid #4CAF50; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.button-group button:hover {
  background-color: #4CAF50;
  color: #fff;
}

.filter-btn {
  padding: 10px 15px;
  background-color: #4CAF50;
  color: #fff;
  border: 1px solid #4CAF50; /* Penambahan border di dalam */
  cursor: pointer;
  border-radius: 5px;
}

.filter-btn:hover {
  background-color: #45a049;
}
</style>
