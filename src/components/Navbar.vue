<template>
  <nav class="navbar">
    <div class="container">
      <div class="navbar-content">
        <h1 class="navbar-brand" @click="goHome">
          TicketFlow
        </h1>

        <div class="navbar-links">
          <template v-if="!isAuthenticated">
            <button class="nav-link" @click="navigate('landing')">Home</button>
            <button class="btn btn-text" @click="navigate('login')">Login</button>
            <button class="btn btn-primary" @click="navigate('signup')">Sign Up</button>
          </template>

          <template v-else>
            <button :class="['nav-link', currentPage === 'dashboard' ? 'active' : '']" @click="navigate('dashboard')">Dashboard</button>
            <button :class="['nav-link', currentPage === 'tickets' ? 'active' : '']" @click="navigate('tickets')">Tickets</button>
            <button class="btn btn-outline" @click="handleLogout">
              <LogOut size="18" />
              Logout
            </button>
          </template>
        </div>

        <button class="mobile-menu-btn" @click="toggleMobile" :aria-expanded="mobileOpen" aria-label="Toggle menu">
          <component :is="mobileOpen ? X : Menu" :size="24" />
        </button>
      </div>

      <div v-if="mobileOpen" class="mobile-menu">
        <template v-if="!isAuthenticated">
          <button class="mobile-nav-link" @click="navigate('landing')">Home</button>
          <button class="mobile-nav-link" @click="navigate('login')">Login</button>
          <button class="mobile-nav-link" @click="navigate('signup')">Sign Up</button>
        </template>
        <template v-else>
          <button :class="['mobile-nav-link', currentPage === 'dashboard' ? 'active' : '']" @click="navigate('dashboard')">Dashboard</button>
          <button :class="['mobile-nav-link', currentPage === 'tickets' ? 'active' : '']" @click="navigate('tickets')">Tickets</button>
          <button class="mobile-nav-link logout" @click="handleLogout">
            <LogOut size="18" />
            Logout
          </button>
        </template>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref } from 'vue';
import { LogOut, X, Menu } from 'lucide-vue-next';

const props = defineProps({
  isAuthenticated: Boolean,
  currentPage: String
});

const emit = defineEmits(['navigate', 'logout']);

const mobileOpen = ref(false);

const toggleMobile = () => mobileOpen.value = !mobileOpen.value;

const navigate = (page) => {
  emit('navigate', page);
  mobileOpen.value = false;
};

const goHome = () => navigate(props.isAuthenticated ? 'dashboard' : 'landing');

const handleLogout = () => {
  emit('logout');
  mobileOpen.value = false;
};
</script>

<style scoped>
.navbar {
  background: white;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: 100;
}
</style>