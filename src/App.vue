<template>
  <div id="app">
    <!-- Navbar -->
    <nav v-if="currentPage !== 'landing'" class="navbar">
      <div class="container">
        <div class="navbar-content">
          <h1 class="navbar-brand" @click="handleNavigate(isAuthenticated ? 'dashboard' : 'landing')">
            TicketFlow
          </h1>

          <!-- Desktop Navigation -->
          <div class="navbar-links">
            <template v-if="!isAuthenticated">
              <button class="nav-link" @click="handleNavigate('landing')">Home</button>
              <button class="btn btn-text" @click="handleNavigate('login')">Login</button>
              <button class="btn btn-primary" @click="handleNavigate('signup')">Sign Up</button>
            </template>
            <template v-else>
              <button 
                :class="['nav-link', { active: currentPage === 'dashboard' }]" 
                @click="handleNavigate('dashboard')"
              >
                Dashboard
              </button>
              <button 
                :class="['nav-link', { active: currentPage === 'tickets' }]" 
                @click="handleNavigate('tickets')"
              >
                Tickets
              </button>
              <button class="btn btn-outline" @click="handleLogout">
                <LogOut :size="18" />
                Logout
              </button>
            </template>
          </div>

          <!-- Mobile Menu Button -->
          <button 
            class="mobile-menu-btn" 
            @click="isMobileMenuOpen = !isMobileMenuOpen"
            :aria-expanded="isMobileMenuOpen"
            aria-label="Toggle menu"
          >
            <X v-if="isMobileMenuOpen" :size="24" />
            <Menu v-else :size="24" />
          </button>
        </div>

        <!-- Mobile Navigation -->
        <div v-if="isMobileMenuOpen" class="mobile-menu">
          <template v-if="!isAuthenticated">
            <button class="mobile-nav-link" @click="handleNavigate('landing')">Home</button>
            <button class="mobile-nav-link" @click="handleNavigate('login')">Login</button>
            <button class="mobile-nav-link" @click="handleNavigate('signup')">Sign Up</button>
          </template>
          <template v-else>
            <button 
              :class="['mobile-nav-link', { active: currentPage === 'dashboard' }]"
              @click="handleNavigate('dashboard')"
            >
              Dashboard
            </button>
            <button 
              :class="['mobile-nav-link', { active: currentPage === 'tickets' }]"
              @click="handleNavigate('tickets')"
            >
              Tickets
            </button>
            <button class="mobile-nav-link logout" @click="handleLogout">
              <LogOut :size="18" />
              Logout
            </button>
          </template>
        </div>
      </div>
    </nav>

    <!-- Toast Notification -->
    <div v-if="toast" :class="['toast', toast.type]">
      <CheckCircle v-if="toast.type === 'success'" :size="20" />
      <AlertCircle v-else :size="20" />
      <span>{{ toast.message }}</span>
      <button @click="toast = null" class="toast-close" aria-label="Close notification">
        <X :size="16" />
      </button>
    </div>

    <!-- Pages -->
    <LandingPage v-if="currentPage === 'landing'" @navigate="handleNavigate" />
    <LoginPage v-if="currentPage === 'login'" @navigate="handleNavigate" @login="handleLogin" @show-toast="showToast" />
    <SignupPage v-if="currentPage === 'signup'" @navigate="handleNavigate" @login="handleLogin" @show-toast="showToast" />
    <Dashboard v-if="currentPage === 'dashboard'" />
    <TicketsPage v-if="currentPage === 'tickets'" @show-toast="showToast" />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import { LogOut, Menu, X, CheckCircle, AlertCircle } from 'lucide-vue-next'
import LandingPage from './components/LandingPage.vue'
import LoginPage from './components/LoginPage.vue'
import SignupPage from './components/SignupPage.vue'
import Dashboard from './components/Dashboard.vue'
import TicketsPage from './components/TicketsPage.vue'

const currentPage = ref('landing')
const isAuthenticated = ref(false)
const isMobileMenuOpen = ref(false)
const toast = ref(null)

onMounted(() => {
  const session = localStorage.getItem('ticketapp_session')
  if (session) {
    isAuthenticated.value = true
    currentPage.value = 'dashboard'
  }
})

// Close mobile menu when page changes
watch(currentPage, () => {
  isMobileMenuOpen.value = false
})

// Auto-hide toast after 3 seconds
watch(toast, (newToast) => {
  if (newToast) {
    setTimeout(() => {
      toast.value = null
    }, 3000)
  }
})

const handleLogin = () => {
  localStorage.setItem('ticketapp_session', JSON.stringify({
    user: 'demo',
    timestamp: Date.now()
  }))
  isAuthenticated.value = true
  currentPage.value = 'dashboard'
}

const handleLogout = () => {
  if (confirm('Are you sure you want to logout?')) {
    localStorage.removeItem('ticketapp_session')
    isAuthenticated.value = false
    currentPage.value = 'landing'
    isMobileMenuOpen.value = false
  }
}

const handleNavigate = (page) => {
  if (['dashboard', 'tickets'].includes(page) && !isAuthenticated.value) {
    currentPage.value = 'login'
    return
  }
  currentPage.value = page
}

const showToast = (message, type = 'success') => {
  toast.value = { message, type }
}
</script>

<style>
/* Global Styles */
* { margin: 0; padding: 0; box-sizing: border-box; }

body { 
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; 
  line-height: 1.6; 
  color: #333; 
}

#app { min-height: 100vh; display: flex; flex-direction: column; }

.page { min-height: 100vh; display: flex; flex-direction: column; }

.container { max-width: 1440px; margin: 0 auto; padding: 0 2rem; width: 100%; }

/* Navbar Styles */
.navbar { 
  background: white; 
  box-shadow: 0 2px 10px rgba(0,0,0,0.1); 
  padding: 1rem 0; 
  position: sticky; 
  top: 0; 
  z-index: 100; 
}

.navbar-content { display: flex; justify-content: space-between; align-items: center; }

.navbar-brand { 
  font-size: 1.5rem; 
  font-weight: 700; 
  color: #667eea; 
  cursor: pointer; 
  transition: color 0.3s; 
}

.navbar-brand:hover { color: #5a67d8; }

.navbar-links { display: flex; gap: 1rem; align-items: center; }

.nav-link { 
  background: none; 
  border: none; 
  color: #4a5568; 
  cursor: pointer; 
  font-size: 1rem; 
  font-weight: 500; 
  padding: 0.5rem 1rem; 
  transition: color 0.3s; 
  position: relative; 
}

.nav-link:hover { color: #667eea; }

.nav-link.active { color: #667eea; }

.nav-link.active::after { 
  content: ''; 
  position: absolute; 
  bottom: 0; 
  left: 50%; 
  transform: translateX(-50%); 
  width: 60%; 
  height: 2px; 
  background: #667eea; 
}

.mobile-menu-btn { 
  display: none; 
  background: none; 
  border: none; 
  cursor: pointer; 
  padding: 0.5rem; 
  color: #4a5568; 
}

.mobile-menu { display: none; }

@media (max-width: 768px) {
  .navbar-links { display: none; }
  .mobile-menu-btn { display: block; }
  .mobile-menu { display: flex; flex-direction: column; gap: 0.5rem; padding: 1rem 0; border-top: 1px solid #e2e8f0; margin-top: 1rem; }
  .mobile-nav-link { background: none; border: none; padding: 1rem; text-align: left; font-size: 1rem; color: #4a5568; cursor: pointer; border-radius: 8px; transition: background 0.3s; display: flex; align-items: center; gap: 0.5rem; }
  .mobile-nav-link:hover { background: #f7fafc; }
  .mobile-nav-link.active { background: #667eea; color: white; }
  .mobile-nav-link.logout { color: #e53e3e; }
}

/* Buttons */
.btn { 
  padding: 0.75rem 1.5rem; 
  border: none; 
  border-radius: 8px; 
  font-size: 1rem; 
  font-weight: 600; 
  cursor: pointer; 
  transition: all 0.3s; 
  display: inline-flex; 
  align-items: center; 
  gap: 0.5rem; 
  font-family: inherit; 
}

.btn-primary { background: #667eea; color: white; }

.btn-primary:hover { 
  background: #5a67d8; 
  transform: translateY(-2px); 
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.3); 
}

.btn-secondary { background: white; color: #667eea; }

.btn-outline { background: transparent; border: 2px solid currentColor; color: #667eea; }

.btn-outline:hover { background: rgba(102, 126, 234, 0.1); }

.btn-text { background: none; border: none; color: #667eea; padding: 0.5rem 1rem; }

.btn-full { width: 100%; justify-content: center; }

/* Toast */
.toast { 
  position: fixed; 
  top: 2rem; 
  right: 2rem; 
  background: white; 
  padding: 1rem 1.5rem; 
  border-radius: 8px; 
  box-shadow: 0 10px 40px rgba(0,0,0,0.2); 
  display: flex; 
  align-items: center; 
  gap: 0.75rem; 
  z-index: 2000; 
  animation: slideIn 0.3s ease; 
  min-width: 300px; 
}

@keyframes slideIn { 
  from { transform: translateX(400px); opacity: 0; } 
  to { transform: translateX(0); opacity: 1; } 
}

.toast.success { border-left: 4px solid #48bb78; }
.toast.error { border-left: 4px solid #e53e3e; }

.toast-close { 
  background: none; 
  border: none; 
  cursor: pointer; 
  padding: 0.25rem; 
  margin-left: auto; 
  color: #718096; 
}

/* Footer */
.footer { 
  background: #2d3748; 
  color: white; 
  padding: 2rem 0; 
  margin-top: auto; 
  text-align: center; 
}

.footer p { margin: 0.25rem 0; opacity: 0.8; font-size: 0.875rem; }

@media (max-width: 768px) {
  .container { padding: 0 1rem; }
  .toast { right: 1rem; left: 1rem; min-width: auto; }
}

*:focus-visible { 
  outline: 3px solid #667eea; 
  outline-offset: 2px; 
}
</style>