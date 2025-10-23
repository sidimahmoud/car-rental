<template>
  <nav class="navbar">
    <div class="nav-container">
      <div class="nav-logo">
        <RouterLink to="/" class="nav-brand">
          <div class="logo-icon">🚗</div>
          <span class="logo-text">CarRent</span>
        </RouterLink>
      </div>

      <div class="nav-menu" :class="{ active: isMenuOpen }">
        <RouterLink to="/" class="nav-link">
          <span>Home</span>
        </RouterLink>
        <RouterLink to="/cars" class="nav-link">
          <span>Browse Cars</span>
        </RouterLink>
        <RouterLink to="/about" class="nav-link">
          <span>About</span>
        </RouterLink>

        <div class="nav-auth" v-if="!user">
          <RouterLink to="/login" class="nav-link nav-login">
            <span>Login</span>
          </RouterLink>
          <RouterLink to="/register" class="nav-link nav-button">
            <span>Sign Up</span>
            <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
            </svg>
          </RouterLink>
        </div>

        <div class="nav-user" v-else>
          <div class="user-info">
            <div class="user-avatar">
              <span>{{ user.email.charAt(0).toUpperCase() }}</span>
            </div>
            <span class="user-name">{{ user.email.split('@')[0] }}</span>
          </div>
          <button @click="logout" class="nav-link nav-button nav-logout">
            <span>Logout</span>
            <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/>
            </svg>
          </button>
        </div>
      </div>

      <div class="nav-toggle" @click="toggleMenu" :class="{ active: isMenuOpen }">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { RouterLink } from 'vue-router'
import { supabase } from '../config/supabase.js'

const isMenuOpen = ref(false)
const user = ref(null)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const logout = async () => {
  await supabase.auth.signOut()
  user.value = null
}

onMounted(async () => {
  const {
    data: { user: currentUser },
  } = await supabase.auth.getUser()
  user.value = currentUser

  supabase.auth.onAuthStateChange((event, session) => {
    user.value = session?.user ?? null
  })
})
</script>

<style scoped>
.navbar {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 1000;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-container {
  max-width: 100%;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 80px;
  width: 100%;
}

.nav-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-decoration: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  padding: 0 1rem;
}

.nav-brand:hover {
  transform: scale(1.05);
}

.logo-icon {
  font-size: 2rem;
  filter: drop-shadow(0 2px 4px rgba(102, 126, 234, 0.3));
}

.logo-text {
  font-size: 1.5rem;
  font-weight: 800;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.nav-menu {
  display: flex;
  align-items: center;
  gap: 2rem;
  padding: 0 1rem;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  text-decoration: none;
  color: #64748b;
  font-weight: 600;
  font-size: 0.95rem;
  border-radius: 12px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.nav-link::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  transition: left 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: -1;
}

.nav-link:hover {
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
}

.nav-link:hover::before {
  left: 0;
}

.nav-link.router-link-active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
}

.nav-login {
  background: transparent;
  border: 2px solid #e2e8f0;
  color: #64748b;
}

.nav-login:hover {
  background: #f8fafc;
  border-color: #667eea;
  color: #667eea;
}

.nav-button {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white !important;
  border: none;
  box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
}

.nav-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 40px rgba(102, 126, 234, 0.4);
}

.nav-logout {
  background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
  box-shadow: 0 10px 25px rgba(239, 68, 68, 0.3);
}

.nav-logout:hover {
  box-shadow: 0 20px 40px rgba(239, 68, 68, 0.4);
}

.btn-icon {
  width: 16px;
  height: 16px;
  transition: transform 0.3s;
}

.nav-link:hover .btn-icon {
  transform: translateX(2px);
}

.nav-auth,
.nav-user {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.user-avatar {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 700;
  font-size: 0.875rem;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3);
}

.user-name {
  color: #1a202c;
  font-weight: 600;
  font-size: 0.875rem;
}

.nav-toggle {
  display: none;
  flex-direction: column;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 8px;
  transition: all 0.3s;
}

.nav-toggle:hover {
  background: #f8fafc;
}

.nav-toggle.active {
  background: #f1f5f9;
}

.bar {
  width: 25px;
  height: 3px;
  background: #64748b;
  margin: 3px 0;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border-radius: 2px;
}

.nav-toggle.active .bar:nth-child(1) {
  transform: rotate(45deg) translate(6px, 6px);
}

.nav-toggle.active .bar:nth-child(2) {
  opacity: 0;
}

.nav-toggle.active .bar:nth-child(3) {
  transform: rotate(-45deg) translate(6px, -6px);
}

@media (max-width: 1024px) {
  .nav-brand {
    padding: 0 1.5rem;
  }
  
  .nav-menu {
    gap: 1.5rem;
    padding: 0 1.5rem;
  }
}

@media (max-width: 768px) {
  .nav-container {
    height: 70px;
  }
  
  .nav-brand {
    padding: 0 1rem;
  }
  
  .nav-menu {
    position: fixed;
    left: -100%;
    top: 70px;
    flex-direction: column;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(20px);
    width: 100%;
    text-align: center;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
    padding: 2rem 0;
    gap: 1rem;
  }

  .nav-menu.active {
    left: 0;
  }

  .nav-toggle {
    display: flex;
  }
  
  .nav-auth,
  .nav-user {
    flex-direction: column;
    gap: 1rem;
    width: 100%;
  }
  
  .nav-link {
    width: 100%;
    justify-content: center;
  }
}
</style>
