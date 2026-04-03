<script setup lang="ts">
import { ref } from 'vue'

interface FormData {
  name: string
  email: string
  message: string
}

interface Errors {
  name?: string
  email?: string
  message?: string
}

const form = ref<FormData>({
  name: '',
  email: '',
  message: '',
})

const errors = ref<Errors>({})
const isSubmitted = ref(false)

const emit = defineEmits<{
    submit: [data: FormData]
}>()

const validateForm = (): boolean => {
    errors.value = {}
    if (!form.value.name.trim()) {
        errors.value.name = 'Name is required'
    }
    if (!form.value.email.trim()) {
        errors.value.email = 'Email is required'
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.value.email)) {
        errors.value.email = 'Please enter a valid email address'
    }
    if (!form.value.message.trim()) {
        errors.value.message = 'Message is required'
    } else if (form.value.message.trim().length < 10) {
        errors.value.message = 'Message must be at least 10 characters long'
    }

    return Object.keys(errors.value).length === 0
}

const handleSubmit = () => {
  if (validateForm()) {
    emit('submit', { ...form.value })
    isSubmitted.value = true
    setTimeout(() => {
      form.value = { name: '', email: '', message: '' }
      isSubmitted.value = false
    }, 2000)
  }
}

const handleReset = () => {
  form.value = { name: '', email: '', message: '' }
  errors.value = {}
}
</script>

<template>
    <div class="form-container">
        <transition name="success">
        <div v-if="isSubmitted" class="success-message">
            ✓ Message sent successfully!
        </div>
        </transition>
        <form @submit.prevent="handleSubmit" class="form">
            <div class="form-group">
                <label for="name" class="form-label">Name</label>
                <input
                    id="name"
                    v-model="form.name"
                    type="text"
                    class="form-input"
                    :class="{ 'form-input-error': errors.name }"
                    placeholder="Your name"
                />
                <span v-if="errors.name" class="error-message">{{ errors.name }}</span>
            </div> 
            <div class="form-group">
                <label for="email" class="form-label">Email</label>
                <input
                    id="email"
                    v-model="form.email"
                    type="email"
                    class="form-input"
                    :class="{ 'form-input-error': errors.email }"
                    placeholder="your@email.com"
                />
                <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
            </div>
            
            <div class="form-group">
                <label for="message" class="form-label">Message</label>
                <textarea
                    id="message"
                    v-model="form.message"
                    class="form-input form-textarea"
                    :class="{ 'form-input-error': errors.message }"
                    placeholder="Your message (min 10 characters)..."
                    rows="5"
                />
                <span v-if="errors.message" class="error-message">{{ errors.message }}</span>
            </div>

            <div class="form-actions">
                <button type="submit" class="submit-btn">Send Message</button>
                <button type="button" class="reset-btn" @click="handleReset">Clear</button>
            </div>
        </form>
    </div>
</template>

<style scoped>
.form-container {
  width: 100%;
  max-width: 500px;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-label {
  font-size: 0.95rem;
  font-weight: 600;
  color: #374151;
}

.form-input {
  padding: 0.75rem 1rem;
  border: 2px solid #e5e7eb;
  border-radius: 8px;
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.2s ease;
  background-color: #ffffff;
}

.form-input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.form-input-error {
  border-color: #ef4444;
}

.form-input-error:focus {
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 120px;
}

.error-message {
  color: #ef4444;
  font-size: 0.875rem;
  font-weight: 500;
}

.form-actions {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.submit-btn {
  flex: 1;
  padding: 0.875rem 1.5rem;
  background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
}

.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(59, 130, 246, 0.4);
}

.submit-btn:active {
  transform: translateY(0);
}

.reset-btn {
  padding: 0.875rem 1.5rem;
  background-color: #f3f4f6;
  color: #374151;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.reset-btn:hover {
  background-color: #e5e7eb;
}

.success-message {
  padding: 1rem;
  background-color: #dcfce7;
  color: #166534;
  border-radius: 8px;
  text-align: center;
  font-weight: 600;
  margin-bottom: 1rem;
  animation: slideDown 0.3s ease;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.success-enter-active,
.success-leave-active {
  transition: all 0.3s ease;
}

.success-enter-from,
.success-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>


        