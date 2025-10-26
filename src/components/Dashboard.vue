<template>
  <div class="page dashboard-page">
    <div class="container">
      <div class="dashboard-header">
        <div>
          <h2>Dashboard</h2>
          <p class="page-description">Overview of your ticket management system</p>
        </div>
      </div>

      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-icon total">📊</div>
          <div class="stat-content">
            <h3>Total Tickets</h3>
            <p class="stat-number">{{ stats.total }}</p>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon open">🟢</div>
          <div class="stat-content">
            <h3>Open Tickets</h3>
            <p class="stat-number">{{ stats.open }}</p>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon progress">🟡</div>
          <div class="stat-content">
            <h3>In Progress</h3>
            <p class="stat-number">{{ stats.inProgress }}</p>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon closed">⚫</div>
          <div class="stat-content">
            <h3>Closed Tickets</h3>
            <p class="stat-number">{{ stats.closed }}</p>
          </div>
        </div>
      </div>

      <div class="decorative-circle circle-dashboard-1"></div>
      <div class="decorative-circle circle-dashboard-2"></div>
    </div>

    <Footer />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import Footer from './Footer.vue'

const tickets = ref([])

onMounted(() => {
  const savedTickets = JSON.parse(localStorage.getItem('tickets') || '[]')
  tickets.value = savedTickets
})

const stats = computed(() => ({
  total: tickets.value.length,
  open: tickets.value.filter(t => t.status === 'open').length,
  inProgress: tickets.value.filter(t => t.status === 'in_progress').length,
  closed: tickets.value.filter(t => t.status === 'closed').length
}))
</script>

<style scoped>
.dashboard-page { background: #f7fafc; }

.dashboard-header { padding: 3rem 0 2rem; }

.dashboard-header h2 { font-size: 2.5rem; color: #2d3748; margin-bottom: 0.5rem; }

.page-description { color: #718096; font-size: 1.125rem; }

.stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 2rem; margin-bottom: 3rem; position: relative; z-index: 1; }

.stat-card { background: white; padding: 2rem; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.08); display: flex; gap: 1.5rem; align-items: center; transition: transform 0.3s; }

.stat-card:hover { transform: translateY(-5px); box-shadow: 0 8px 30px rgba(0,0,0,0.12); }

.stat-icon { font-size: 3rem; width: 60px; height: 60px; display: flex; align-items: center; justify-content: center; border-radius: 12px; flex-shrink: 0; }

.stat-icon.total { background: #e6f2ff; }
.stat-icon.open { background: #d4f4dd; }
.stat-icon.progress { background: #fff4cc; }
.stat-icon.closed { background: #e2e8f0; }

.stat-content h3 { font-size: 1rem; color: #718096; margin-bottom: 0.25rem; font-weight: 600; }

.stat-number { font-size: 2.5rem; font-weight: 700; color: #2d3748; }

.decorative-circle { position: absolute; border-radius: 50%; opacity: 0.1; }

.circle-dashboard-1 { width: 150px; height: 150px; top: 20%; right: 5%; background: #667eea; }

.circle-dashboard-2 { width: 100px; height: 100px; bottom: 30%; left: 10%; background: #764ba2; }

@media (max-width: 768px) {
  .stats-grid { grid-template-columns: 1fr; }
  .dashboard-header h2 { font-size: 2rem; }
}
</style>