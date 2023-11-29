<template>
  <div class="todo-container">
    <!-- Navigation links -->
    <h1 class="title">Todo List</h1>
    <router-link to="/create" class="nav-link">Create Todo</router-link>

    <!-- Read section -->
    <div v-if="currentView === 'read'" class="todo-read">
      <div class="add-todo-section">
        <input type="text" v-model="newTodo" placeholder="Enter new todo" class="input-field">
        <button @click="addTodo" class="btn-add">Add Todo</button>
      </div>
      <h2 class="section-title">Todos List</h2>
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          {{ todo }}
          <button @click="editTodo(index)" class="btn-edit">Edit</button>
          <button @click="confirmDelete(index)" class="btn-delete">Delete</button>
        </li>
      </ul>
    </div>

    <!-- Create section -->
    <div v-if="currentView === 'create'" class="todo-create">
      <h2 class="section-title">Create Todo</h2>
      <input type="text" v-model="newTodo" placeholder="Enter new todo" class="input-field">
      <button @click="addTodo" class="btn-add">Add Todo</button>
    </div>

    <!-- Update section -->
    <div v-if="currentView === 'update'" class="todo-update">
      <h2 class="section-title">Update Todo</h2>
      <input type="text" v-model="updatedTodo" placeholder="Enter updated todo" class="input-field">
      <button @click="updateTodo" class="btn-update">Update Todo</button>
    </div>

    <!-- Delete confirmation dialog -->
    <div v-if="currentView === 'confirmDelete'" class="todo-delete-confirm">
      <h2 class="section-title">Confirm Delete</h2>
      <p>Are you sure you want to delete this todo?</p>
      <button @click="deleteTodo" class="btn-delete-confirm">Yes, Delete</button>
      <button @click="cancelDelete" class="btn-cancel-delete">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentView: 'read',
      todos: ['Todo 1', 'Todo 2', 'Todo 3'],
      newTodo: '',
      updatedTodo: '',
      deleteIndex: -1,
    };
  },
  methods: {
    // Method to add a new todo
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push(this.newTodo);
        this.newTodo = '';
        if (this.currentView !== 'update') {
          this.currentView = 'read';
        }
      }
    },
    // Method to switch to update view and populate with selected todo
    editTodo(index) {
      this.updatedTodo = this.todos[index];
      this.deleteIndex = index; // Store the index for updating
      this.currentView = 'update';
    },
    // Method to update todo and switch back to read view
    updateTodo() {
      if (this.updatedTodo.trim() !== '') {
        this.todos[this.deleteIndex] = this.updatedTodo;
        this.updatedTodo = '';
        this.currentView = 'read';
      }
    },
    // Method to delete a todo
    deleteTodo() {
      this.todos.splice(this.deleteIndex, 1);
      this.currentView = 'read';
    },
    // Method to confirm delete
    confirmDelete(index) {
      this.deleteIndex = index;
      this.currentView = 'confirmDelete';
    },
    // Method to cancel delete
    cancelDelete() {
      this.currentView = 'read';
    },
  },
};
</script>



<style scoped>
/* Container styles */
.todo-container {
  max-width: 800px;
  height: auto;
  margin: 0 auto;
  font-family: 'Arial', sans-serif;
  color: #333;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

/* Title styles */
.title {
  text-align: center;
  font-size: 28px;
  margin-bottom: 20px;
  color: #2c3e50;
}

/* Navigation link styles */
.nav-link {
  display: block;
  margin-bottom: 10px;
  text-decoration: none;
  color: #3498db;
  transition: color 0.3s ease;
}
.nav-link:hover {
  color: #2980b9;
}

/* List styles */
.todo-list {
  list-style: none;
  padding: 0;
}

/* Item styles */
.todo-item {
  margin-bottom: 15px;
  padding: 12px;
  background-color: #fff;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.1);
}
.todo-item:hover {
  transform: translateY(-2px);
}

/* Section title styles */
.section-title {
  margin-top: 30px;
  margin-bottom: 15px;
  font-size: 22px;
  color: #34495e;
}

/* Input field styles */
.input-field {
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
}

/* Button styles */
.btn-add,
.btn-update,
.btn-delete,
.btn-edit {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: #fff;
  transition: background-color 0.3s ease;
}
.btn-add {
  background-color: #27ae60;
}
.btn-update {
  background-color: #f39c12;
}
.btn-delete {
  background-color: #c0392b;
}
.btn-delete-confirm,
.btn-cancel-delete{
  background-color: #3498db;
  transition: background-color 0.3s ease;
  border-radius: 4px;
  border: none;
  box-shadow: none;
  padding: 8px 16px;
  margin-right: 10px;
}
.btn-edit {
  background-color: #2980b9;
}

/* Hover styles for buttons */
.btn-add:hover,
.btn-update:hover,
.btn-delete:hover,
.btn-edit:hover {
  filter: brightness(90%);
}
</style>
