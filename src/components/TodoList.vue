<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Todo {
  id: number
  title: string
  completed: boolean
}

interface ApiTodo {
  userId: number
  id: number
  title: string
  completed: boolean
}

const todos = ref<Todo[]>([])
const loading = ref(true)
const error = ref<string | null>(null)
const newTodoTitle = ref('')

const fetchTodos = async () => {
  try {
    loading.value = true
    error.value = null
    const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const data: ApiTodo[] = await response.json()
    todos.value = data.map((todo) => ({
      id: todo.id,
      title: todo.title,
      completed: todo.completed,
    }))
  } catch (err) {
    error.value = 'Failed to fetch todos. Please try again later.'
    console.error('Error fetching todos:', err)
  } finally {
    loading.value = false
  }
}

const addTodo = () => {
  if (newTodoTitle.value.trim()) {
    const newTodo: Todo = {
      id: Math.max(...todos.value.map((t) => t.id), 0) + 1,
      title: newTodoTitle.value.trim(),
      completed: false,
    }
    todos.value.push(newTodo)
    newTodoTitle.value = ''
  }
}

const deleteTodo = (id: number) => {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

const toggleTodo = (id: number) => {
  const todo = todos.value.find((t) => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const handleAddKeyPress = (e: KeyboardEvent) => {
  if (e.key === 'Enter') {
    addTodo()
  }
}

onMounted(() => {
  fetchTodos()
})
</script>

<template>
  <div class="todo-container">
    <div class="todo-card">
      <h2 class="todo-title">My Todo List</h2>

      <!-- Loading State -->
      <div v-if="loading" class="loading-state">
        <div class="spinner"></div>
        <p>Loading todos...</p>
      </div>

      <!-- Error State -->
      <div v-else-if="error" class="error-state">
        <span class="error-icon">⚠</span>
        <p>{{ error }}</p>
        <button class="retry-btn" @click="fetchTodos">Retry</button>
      </div>

      <!-- Todo List -->
      <div v-else class="todo-content">
        <!-- Add New Todo -->
        <div class="add-todo-section">
          <input
            v-model="newTodoTitle"
            type="text"
            class="todo-input"
            placeholder="Add a new todo..."
            @keypress="handleAddKeyPress"
          />
          <button class="add-btn" @click="addTodo">Add</button>
        </div>

        <!-- Todo Items -->
        <ul v-if="todos.length > 0" class="todo-list">
          <li v-for="todo in todos" :key="todo.id" class="todo-item">
            <div class="todo-item-left">
              <input
                type="checkbox"
                :id="`todo-${todo.id}`"
                :checked="todo.completed"
                class="todo-checkbox"
                @change="toggleTodo(todo.id)"
              />
              <label :for="`todo-${todo.id}`" class="todo-label">
                <span class="todo-text" :class="{ 'todo-completed': todo.completed }">
                  {{ todo.title }}
                </span>
              </label>
            </div>
            <button class="delete-btn" @click="deleteTodo(todo.id)" aria-label="Delete todo">
              ✕
            </button>
          </li>
        </ul>

        <!-- Empty State -->
        <div v-else class="empty-state">
          <span class="empty-icon">✓</span>
          <p>All todos completed! Great job!</p>
        </div>

        <!-- Stats -->
        <div v-if="todos.length > 0" class="todo-stats">
          <p class="stat">Completed: {{ todos.filter((t) => t.completed).length }} / {{ todos.length }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.todo-container {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  min-height: 600px;
}

.todo-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
  overflow: hidden;
}

.todo-title {
  font-size: 1.875rem;
  font-weight: 700;
  color: #1f2937;
  margin: 0;
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-content {
  padding: 1.5rem;
}

/* Loading State */
.loading-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 3rem 1.5rem;
  gap: 1rem;
  color: #6b7280;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top-color: #667eea;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Error State */
.error-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  gap: 1rem;
  text-align: center;
  color: #dc2626;
}

.error-icon {
  font-size: 2.5rem;
}

.retry-btn {
  padding: 0.5rem 1rem;
  background-color: #dc2626;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.2s ease;
}

.retry-btn:hover {
  background-color: #b91c1c;
}

/* Add Todo Section */
.add-todo-section {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
}

.todo-input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 2px solid #e5e7eb;
  border-radius: 8px;
  font-size: 0.95rem;
  font-family: inherit;
  transition: all 0.2s ease;
}

.todo-input:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.add-btn {
  padding: 0.75rem 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.add-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

/* Todo List */
.todo-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  background-color: #f9fafb;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
  transition: all 0.2s ease;
}

.todo-item:hover {
  background-color: #f3f4f6;
  border-color: #d1d5db;
}

.todo-item-left {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #667eea;
}

.todo-label {
  cursor: pointer;
  flex: 1;
}

.todo-text {
  color: #1f2937;
  font-size: 0.95rem;
  transition: all 0.2s ease;
}

.todo-completed {
  text-decoration: line-through;
  color: #9ca3af;
}

.delete-btn {
  background: none;
  border: none;
  color: #ef4444;
  cursor: pointer;
  font-size: 1.25rem;
  padding: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.delete-btn:hover {
  background-color: #fee2e2;
  color: #dc2626;
}

/* Empty State */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  gap: 0.5rem;
  color: #6b7280;
  text-align: center;
}

.empty-icon {
  font-size: 2.5rem;
}

/* Stats */
.todo-stats {
  margin-top: 1.5rem;
  padding-top: 1.5rem;
  border-top: 1px solid #e5e7eb;
}

.stat {
  color: #6b7280;
  font-size: 0.9rem;
  margin: 0;
  font-weight: 500;
}

@media (max-width: 480px) {
  .todo-container {
    padding: 1rem;
    min-height: auto;
  }

  .todo-card {
    max-width: 100%;
  }

  .add-todo-section {
    flex-direction: column;
  }

  .add-btn {
    width: 100%;
  }
}
</style>
