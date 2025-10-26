<template>
  <div class="page auth-page">
    <div class="auth-container">
      <div class="auth-card">
        <h2>Create Account</h2>
        <p class="auth-subtitle">Join TicketFlow today</p>
        
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="name">Full Name</label>
            <input
              type="text"
              id="name"
              v-model="formData.name"
              @blur="validateName"
              @input="errors.name = ''"
              :class="{ error: errors.name }"
            />
            <span v-if="errors.name" class="error-message" role="alert">
              {{ errors.name }}
            </span>
          </div>

          <div class="form-group">
            <label for="email">Email Address</label>
            <input
              type="email"
              id="email"
              v-model="formData.email"
              @blur="validateEmail"
              @input="errors.email = ''"
              :class="{ error: errors.email }"
            />
            <span v-if="errors.email" class="error-message" role="alert">
              {{ errors.email }}
            </span>
          </div>

          <div class="form-group">
            <label for="password">Password</label>
            <input
              type="password"
              id="password"
              v-model="formData.password"
              @blur="validatePassword"
              @input="errors.password = ''"
              :class="{ error: errors.password }"
            />
            <span v-if="errors.password" class="error-message" role="alert">
              {{ errors.password }}
            </span>
          </div>

          <div class="form-group">
            <label for="confirmPassword">Confirm Password</label>
            <input
              type="password"
              id="confirmPassword"
              v-model="formData.confirmPassword"
              @blur="validateConfirmPassword"
              @input="errors.confirmPassword = ''"
              :class="{ error: errors.confirmPassword }"
            />
            <span v-if="errors.confirmPassword" class="error-message" role="alert">
              {{ errors.confirmPassword }}
            </span>
          </div>

          <button type="submit" class="btn btn-primary btn-full">
            Sign Up
          </button>
        </form>

        <p class="auth-footer">
          Already have an account?
          <button class="link-btn" @click="$emit('navigate', 'login')">
            Log In
          </button>
        </p>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import Footer from './Footer.vue'

const emit = defineEmits(['navigate', 'login', 'show-toast'])

const formData = reactive({
  name: '',
  email: '',
  password: '',
  confirmPassword: ''
})

const errors = reactive({
  name: '',
  email: '',
  password: '',
  confirmPassword: ''
})

const validateName = () => {
  if (!formData.name) {
    errors.name = 'Name is required'
    return false
  }
  errors.name = ''
  return true
}

const validateEmail = () => {
  if (!formData.email) {
    errors.email = 'Email is required'
    return false
  }
  if (!/\S+@\S+\.\S+/.test(formData.email)) {
    errors.email = 'Invalid email format'
    return false
  }
  errors.email = ''
  return true
}

const validatePassword = () => {
  if (!formData.password) {
    errors.password = 'Password is required'
    return false
  }
  if (formData.password.length < 6) {
    errors.password = 'Password must be at least 6 characters'
    return false
  }
  errors.password = ''
  return true
}

const validateConfirmPassword = () => {
  if (formData.password !== formData.confirmPassword) {
    errors.confirmPassword = 'Passwords do not match'
    return false
  }
  errors.confirmPassword = ''
  return true
}

const handleSubmit = () => {
  const isNameValid = validateName()
  const isEmailValid = validateEmail()
  const isPasswordValid = validatePassword()
  const isConfirmValid = validateConfirmPassword()
  
  if (!isNameValid || !isEmailValid || !isPasswordValid || !isConfirmValid) {
    return
  }

  emit('show-toast', 'Account created successfully!', 'success')
  setTimeout(() => {
    emit('login')
  }, 1500)
}
</script>

<style scoped>
.auth-page { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); display: flex; flex-direction: column; flex: 1; padding: 2rem 0; }

.auth-container { flex: 1; display: flex; align-items: center; justify-content: center; padding: 2rem 0; }

.auth-card { background: white; padding: 3rem; border-radius: 16px; box-shadow: 0 10px 40px rgba(0,0,0,0.2); max-width: 450px; width: 100%; }

.auth-card h2 { font-size: 2rem; margin-bottom: 0.5rem; color: #2d3748; }

.auth-subtitle { color: #718096; margin-bottom: 2rem; }

.form-group { margin-bottom: 1.5rem; }

.form-group label { display: block; font-weight: 600; margin-bottom: 0.5rem; color: #2d3748; }

.form-group input { width: 100%; padding: 0.75rem; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 1rem; font-family: inherit; transition: all 0.2s; }

.form-group input:focus { outline: none; border-color: #667eea; box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1); }

.form-group input.error { border-color: #e53e3e; }

.error-message { color: #e53e3e; font-size: 0.875rem; margin-top: 0.25rem; display: block; }

.auth-footer { text-align: center; margin-top: 1.5rem; color: #718096; }

.link-btn { background: none; border: none; color: #667eea; cursor: pointer; font-weight: 600; text-decoration: underline; font-family: inherit; }

@media (max-width: 768px) {
  .auth-card { padding: 2rem 1.5rem; }
}
</style>
