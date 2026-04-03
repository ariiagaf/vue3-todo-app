<script setup lang="ts">
import { ref } from 'vue'
import Counter from './components/Counter.vue'
import Modal from './components/Modal.vue'
import TodoList from './components/TodoList.vue'
import Form from './components/Form.vue'

interface FormData {
  name: string
  email: string
  message: string
}

const isModalOpen = ref(false)
const submittedFormData = ref<FormData | null>(null)

const openModal = () => {
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
}

const handleFormSubmit = (data: FormData) => {
  submittedFormData.value = data
  console.log('Form submitted:', data)
  closeModal()
}
</script>

<template>
  <div class="app-wrapper">
    <!-- Header -->
    <header class="app-header">
      <div class="header-content">
        <h1 class="app-title">Vue 3 Component Showcase</h1>
        <p class="app-subtitle">Beautiful Components with TypeScript & Modern Styling</p>
      </div>
    </header>

    <!-- Main Content -->
    <main class="app-main">
      <!-- Counter Section -->
      <section class="section counter-section">
        <Counter />
      </section>

      <!-- Todo List Section -->
      <section class="section todo-section">
        <TodoList />
      </section>

      <!-- Contact Form Section -->
      <section class="section form-section">
        <div class="form-wrapper">
          <h2 class="section-title">Contact Us</h2>
          <p class="section-description">Have questions? Send us a message!</p>
          <Form @submit="handleFormSubmit" />
          <button class="modal-trigger-btn" @click="openModal">
            Or open form in modal →
          </button>
        </div>
      </section>
    </main>

    <!-- Modal with Form -->
    <Modal :is-open="isModalOpen" title="Send Message" @close="closeModal">
      <Form @submit="handleFormSubmit" />
    </Modal>

    <!-- Footer -->
    <footer class="app-footer">
      <p class="footer-text">
        Built with <span class="heart">♥</span> using Vue 3, TypeScript, and modern CSS
      </p>
      <div v-if="submittedFormData" class="footer-info">
        <p class="last-submission">Last submission: {{ submittedFormData.name }} ({{ submittedFormData.email }})</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app-wrapper {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  display: flex;
  flex-direction: column;
}

.app-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 3rem 1rem;
  text-align: center;
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.2);
  position: relative;
  overflow: hidden;
}

.app-header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
  pointer-events: none;
}

.header-content {
  position: relative;
  z-index: 1;
}

.app-title {
  font-size: 2.5rem;
  font-weight: 800;
  margin-bottom: 0.5rem;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.app-subtitle {
  font-size: 1.1rem;
  opacity: 0.95;
  letter-spacing: 0.5px;
}

.app-main {
  flex: 1;
  padding: 3rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.section {
  animation: fadeInUp 0.6s ease-out;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.counter-section,
.todo-section {
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-section {
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-wrapper {
  background: white;
  border-radius: 20px;
  padding: 3rem;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.1);
}

.section-title {
  font-size: 1.875rem;
  font-weight: 700;
  color: #1f2937;
  margin-bottom: 0.5rem;
}

.section-description {
  font-size: 1rem;
  color: #6b7280;
  margin-bottom: 2rem;
}

.modal-trigger-btn {
  width: 100%;
  margin-top: 1rem;
  padding: 0.875rem 1.5rem;
  background-color: #f3f4f6;
  color: #374151;
  border: 2px solid #e5e7eb;
  border-radius: 8px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.modal-trigger-btn:hover {
  background-color: #e5e7eb;
  border-color: #d1d5db;
  transform: translateY(-2px);
}

.app-footer {
  background: linear-gradient(135deg, #1f2937 0%, #111827 100%);
  color: white;
  padding: 2rem 1rem;
  text-align: center;
  margin-top: auto;
}

.footer-text {
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
}

.heart {
  color: #ef4444;
  display: inline-block;
  animation: heartbeat 1.5s ease-in-out infinite;
}

@keyframes heartbeat {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
}

.footer-info {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
}

.last-submission {
  font-size: 0.85rem;
  opacity: 0.8;
  margin: 0;
}

/* Responsive Design */
@media (max-width: 768px) {
  .app-header {
    padding: 2rem 1rem;
  }

  .app-title {
    font-size: 1.875rem;
  }

  .app-subtitle {
    font-size: 1rem;
  }

  .app-main {
    padding: 2rem 1rem;
    gap: 2rem;
  }

  .form-wrapper {
    padding: 2rem;
  }

  .section-title {
    font-size: 1.5rem;
  }
}

@media (max-width: 480px) {
  .app-header {
    padding: 1.5rem 1rem;
  }

  .app-title {
    font-size: 1.5rem;
  }

  .app-subtitle {
    font-size: 0.95rem;
  }

  .app-main {
    padding: 1.5rem 0.75rem;
    gap: 1.5rem;
  }

  .form-wrapper {
    padding: 1.5rem;
    border-radius: 12px;
  }

  .section-title {
    font-size: 1.25rem;
  }
}
</style>
