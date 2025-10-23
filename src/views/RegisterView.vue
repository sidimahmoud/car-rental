<template>
  <div class="register-page">
    <div class="register-container">
      <div class="register-form">
        <h1>Create Account</h1>
        <p>Sign up to start renting cars</p>

        <form @submit.prevent="handleRegister">
          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="email"
              id="email"
              v-model="email"
              required
              placeholder="Enter your email"
            />
          </div>

          <div class="form-group">
            <label for="password">Password</label>
            <input
              type="password"
              id="password"
              v-model="password"
              required
              placeholder="Create a password"
              minlength="6"
            />
          </div>

          <div class="form-group">
            <label for="confirmPassword">Confirm Password</label>
            <input
              type="password"
              id="confirmPassword"
              v-model="confirmPassword"
              required
              placeholder="Confirm your password"
            />
          </div>

          <button type="submit" class="btn btn-primary" :disabled="loading">
            {{ loading ? 'Creating Account...' : 'Sign Up' }}
          </button>
        </form>

        <div class="form-footer">
          <p>Already have an account? <RouterLink to="/login">Sign in</RouterLink></p>
        </div>

        <div v-if="error" class="error-message">
          {{ error }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { RouterLink, useRouter } from 'vue-router'
import { supabase } from '../config/supabase.js'

const router = useRouter()
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const loading = ref(false)
const error = ref('')

const handleRegister = async () => {
  if (password.value !== confirmPassword.value) {
    error.value = 'Passwords do not match'
    return
  }

  if (password.value.length < 6) {
    error.value = 'Password must be at least 6 characters'
    return
  }

  loading.value = true
  error.value = ''

  try {
    const { data, error: authError } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
    })

    if (authError) {
      error.value = authError.message
    } else {
      router.push('/')
    }
  } catch (err) {
    error.value = 'An unexpected error occurred'
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.register-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  margin-top: 70px;
}

.register-container {
  background: white;
  border-radius: 10px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  width: 100%;
  max-width: 400px;
  margin: 20px;
}

.register-form h1 {
  text-align: center;
  margin-bottom: 0.5rem;
  color: #1f2937;
}

.register-form p {
  text-align: center;
  color: #6b7280;
  margin-bottom: 2rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #374151;
  font-weight: 500;
}

.form-group input {
  width: 100%;
  padding: 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.form-group input:focus {
  outline: none;
  border-color: #2563eb;
}

.btn {
  width: 100%;
  padding: 12px;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-primary {
  background: #2563eb;
  color: white;
}

.btn-primary:hover:not(:disabled) {
  background: #1d4ed8;
}

.btn-primary:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.form-footer {
  text-align: center;
  margin-top: 1.5rem;
}

.form-footer a {
  color: #2563eb;
  text-decoration: none;
}

.form-footer a:hover {
  text-decoration: underline;
}

.error-message {
  background: #fef2f2;
  color: #dc2626;
  padding: 12px;
  border-radius: 6px;
  margin-top: 1rem;
  text-align: center;
}
</style>
