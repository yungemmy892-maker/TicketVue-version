<template>
  <div class="page">
    <!-- Hero Section -->
    <section class="hero">
      <div class="container">
        <div class="hero-content">
          <div class="decorative-circle circle-1"></div>
          <div class="decorative-circle circle-2"></div>
          <h1 class="hero-title">TicketFlow</h1>
          <p class="hero-subtitle">
            Manage your support tickets efficiently with our intuitive platform
          </p>
          <div class="hero-buttons">
            <button class="btn btn-primary" @click="$emit('navigate', 'login')">
              Login
            </button>
            <button class="btn btn-secondary" @click="$emit('navigate', 'signup')">
              Get Started
            </button>
          </div>
        </div>
      </div>
      <WaveSVG />
    </section>

    <!-- Features Section -->
    <section class="features">
      <div class="container">
        <h2 class="section-title">Why Choose TicketFlow?</h2>
        <div class="feature-grid">
          <div class="feature-card">
            <div class="feature-icon">🎯</div>
            <h3>Easy Management</h3>
            <p>Create, update, and track tickets with ease</p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">⚡</div>
            <h3>Real-time Updates</h3>
            <p>Get instant notifications on ticket status changes</p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">🔒</div>
            <h3>Secure & Private</h3>
            <p>Your data is protected with industry-standard security</p>
          </div>
        </div>
      </div>
    </section>

    <Footer />
  </div>
</template>

<script setup>
import WaveSVG from './WaveSVG.vue'
import Footer from './Footer.vue'

defineEmits(['navigate'])
</script>

<style scoped>
.hero { 
  position: relative; 
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
  color: white; 
  padding: 8rem 0 12rem; 
  overflow: hidden; 
}

.hero-content { position: relative; z-index: 2; text-align: center; }

.hero-title { font-size: 4rem; font-weight: 700; margin-bottom: 1rem; }

.hero-subtitle { font-size: 1.5rem; margin-bottom: 2rem; opacity: 0.9; }

.hero-buttons { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; }

.decorative-circle { position: absolute; border-radius: 50%; opacity: 0.1; background: white; }

.circle-1 { width: 300px; height: 300px; top: 10%; right: 10%; }

.circle-2 { width: 200px; height: 200px; bottom: 20%; left: 5%; }

.features { padding: 5rem 0; background: white; }

.section-title { font-size: 2.5rem; text-align: center; margin-bottom: 3rem; color: #2d3748; }

.feature-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 2rem; }

.feature-card { background: white; padding: 2rem; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.1); text-align: center; transition: transform 0.3s; }

.feature-card:hover { transform: translateY(-5px); }

.feature-icon { font-size: 3rem; margin-bottom: 1rem; }

.feature-card h3 { font-size: 1.5rem; margin-bottom: 0.5rem; color: #2d3748; }

.feature-card p { color: #718096; }

@media (max-width: 768px) {
  .hero-title { font-size: 2.5rem; }
  .hero-subtitle { font-size: 1.2rem; }
  .hero { padding: 5rem 0 8rem; }
  .feature-grid { grid-template-columns: 1fr; }
}
</style>