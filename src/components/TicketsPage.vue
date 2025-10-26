<template>
  <div class="page tickets-page">
    <div class="container">
      <div class="tickets-header">
        <div>
          <h2>Ticket Management</h2>
          <p class="page-description">Create, view, edit, and manage all your tickets</p>
        </div>
        <button class="btn btn-primary" @click="showModal = true">
          <Plus :size="18" />
          New Ticket
        </button>
      </div>

      <div v-if="tickets.length === 0" class="empty-state">
        <Ticket :size="64" />
        <h3>No tickets yet</h3>
        <p>Create your first ticket to get started</p>
      </div>

      <div v-else class="tickets-grid">
        <div v-for="ticket in tickets" :key="ticket.id" class="ticket-card">
          <div class="ticket-header">
            <h3>{{ ticket.title }}</h3>
            <span :class="['status-badge', getStatusClass(ticket.status)]">
              {{ ticket.status.replace('_', ' ') }}
            </span>
          </div>
          <p class="ticket-description">{{ ticket.description || 'No description' }}</p>
          <div class="ticket-meta">
            <span class="priority">Priority: {{ ticket.priority }}</span>
            <span class="date">{{ formatDate(ticket.createdAt) }}</span>
          </div>
          <div class="ticket-actions">
            <button class="btn-icon" @click="handleEdit(ticket)" title="Edit" aria-label="Edit ticket">
              <Edit2 :size="18" />
            </button>
            <button class="btn-icon delete" @click="handleDelete(ticket.id)" title="Delete" aria-label="Delete ticket">
              <Trash2 :size="18" />
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div v-if="showModal" class="modal-overlay" @click="resetForm">
      <div class="modal" @click.stop>
        <div class="modal-header">
          <h3>{{ editingTicket ? 'Edit Ticket' : 'Create New Ticket' }}</h3>
          <button class="btn-icon" @click="resetForm" aria-label="Close modal">
            <X :size="20" />
          </button>
        </div>

        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="title">Title *</label>
            <input
              type="text"
              id="title"
              v-model="formData.title"
              @blur="validateTitle"
              @input="errors.title = ''"
              :class="{ error: errors.title }"
              :aria-describedby="errors.title ? 'title-error' : undefined"
            />
            <span v-if="errors.title" class="error-message" id="title-error" role="alert">
              {{ errors.title }}
            </span>
          </div>

          <div class="form-group">
            <label for="description">Description</label>
            <textarea
              id="description"
              v-model="formData.description"
              @blur="validateDescription"
              @input="errors.description = ''"
              rows="4"
              :class="{ error: errors.description }"
              :aria-describedby="errors.description ? 'description-error' : undefined"
            ></textarea>
            <span v-if="errors.description" class="error-message" id="description-error" role="alert">
              {{ errors.description }}
            </span>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="status">Status *</label>
              <select
                id="status"
                v-model="formData.status"
                @change="errors.status = ''"
                :class="{ error: errors.status }"
                :aria-describedby="errors.status ? 'status-error' : undefined"
              >
                <option value="open">Open</option>
                <option value="in_progress">In Progress</option>
                <option value="closed">Closed</option>
              </select>
              <span v-if="errors.status" class="error-message" id="status-error" role="alert">
                {{ errors.status }}
              </span>
            </div>

            <div class="form-group">
              <label for="priority">Priority</label>
              <select
                id="priority"
                v-model="formData.priority"
              >
                <option value="low">Low</option>
                <option value="medium">Medium</option>
                <option value="high">High</option>
              </select>
            </div>
          </div>

          <div class="modal-actions">
            <button type="button" class="btn btn-outline" @click="resetForm">
              Cancel
            </button>
            <button type="submit" class="btn btn-primary">
              {{ editingTicket ? 'Update' : 'Create' }} Ticket
            </button>
          </div>
        </form>
      </div>
    </div>

    <Footer />
  </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import { Plus, Edit2, Trash2, X, Ticket } from 'lucide-vue-next'
import Footer from './Footer.vue'

const emit = defineEmits(['show-toast'])

const tickets = ref([])
const showModal = ref(false)
const editingTicket = ref(null)

const formData = reactive({
  title: '',
  description: '',
  status: 'open',
  priority: 'medium'
})

const errors = reactive({
  title: '',
  description: '',
  status: ''
})

onMounted(() => {
  loadTickets()
})

const loadTickets = () => {
  const saved = JSON.parse(localStorage.getItem('tickets') || '[]')
  tickets.value = saved
}

const saveTickets = (newTickets) => {
  localStorage.setItem('tickets', JSON.stringify(newTickets))
  tickets.value = newTickets
}

const validateTitle = () => {
  if (!formData.title.trim()) {
    errors.title = 'Title is required'
    return false
  }
  if (formData.title.trim().length < 3) {
    errors.title = 'Title must be at least 3 characters'
    return false
  }
  errors.title = ''
  return true
}

const validateDescription = () => {
  if (formData.description && formData.description.length > 500) {
    errors.description = 'Description must be less than 500 characters'
    return false
  }
  errors.description = ''
  return true
}

const validateStatus = () => {
  if (!['open', 'in_progress', 'closed'].includes(formData.status)) {
    errors.status = 'Status must be: open, in_progress, or closed'
    return false
  }
  errors.status = ''
  return true
}

const handleSubmit = () => {
  const isTitleValid = validateTitle()
  const isDescriptionValid = validateDescription()
  const isStatusValid = validateStatus()
  
  if (!isTitleValid || !isDescriptionValid || !isStatusValid) {
    return
  }

  if (editingTicket.value) {
    const updated = tickets.value.map(t => 
      t.id === editingTicket.value.id 
        ? { ...formData, id: t.id, createdAt: t.createdAt }
        : t
    )
    saveTickets(updated)
    emit('show-toast', 'Ticket updated successfully!', 'success')
  } else {
    const newTicket = {
      ...formData,
      id: Date.now(),
      createdAt: new Date().toISOString()
    }
    saveTickets([...tickets.value, newTicket])
    emit('show-toast', 'Ticket created successfully!', 'success')
  }

  resetForm()
}

const handleEdit = (ticket) => {
  editingTicket.value = ticket
  Object.assign(formData, {
    title: ticket.title,
    description: ticket.description,
    status: ticket.status,
    priority: ticket.priority
  })
  showModal.value = true
}

const handleDelete = (id) => {
  if (confirm('Are you sure you want to delete this ticket?')) {
    saveTickets(tickets.value.filter(t => t.id !== id))
    emit('show-toast', 'Ticket deleted successfully!', 'success')
  }
}

const resetForm = () => {
  Object.assign(formData, {
    title: '',
    description: '',
    status: 'open',
    priority: 'medium'
  })
  Object.assign(errors, {
    title: '',
    description: '',
    status: ''
  })
  editingTicket.value = null
  showModal.value = false
}

const getStatusClass = (status) => {
  const classes = {
    open: 'status-open',
    in_progress: 'status-progress',
    closed: 'status-closed'
  }
  return classes[status] || ''
}

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleDateString()
}
</script>

<style scoped>
.tickets-page { background: #f7fafc; }

.tickets-header { display: flex; justify-content: space-between; align-items: center; padding: 3rem 0 2rem; flex-wrap: wrap; gap: 1rem; }

.tickets-header h2 { font-size: 2.5rem; color: #2d3748; margin-bottom: 0.5rem; }

.page-description { color: #718096; font-size: 1.125rem; }

.tickets-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(350px, 1fr)); gap: 1.5rem; padding-bottom: 3rem; }

.ticket-card { background: white; padding: 1.5rem; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.08); transition: all 0.3s; position: relative; }

.ticket-card:hover { transform: translateY(-5px); box-shadow: 0 8px 25px rgba(0,0,0,0.12); }

.ticket-header { display: flex; justify-content: space-between; align-items: start; margin-bottom: 1rem; gap: 1rem; }

.ticket-header h3 { font-size: 1.25rem; color: #2d3748; margin: 0; flex: 1; word-break: break-word; }

.status-badge { padding: 0.25rem 0.75rem; border-radius: 20px; font-size: 0.75rem; font-weight: 600; text-transform: capitalize; white-space: nowrap; }

.status-open { background: #d4f4dd; color: #22543d; }
.status-progress { background: #fff4cc; color: #975a16; }
.status-closed { background: #e2e8f0; color: #2d3748; }

.ticket-description { color: #718096; margin-bottom: 1rem; line-height: 1.6; }

.ticket-meta { display: flex; justify-content: space-between; align-items: center; font-size: 0.875rem; color: #718096; margin-bottom: 1rem; padding-top: 1rem; border-top: 1px solid #e2e8f0; }

.priority { text-transform: capitalize; font-weight: 600; }

.ticket-actions { display: flex; gap: 0.5rem; justify-content: flex-end; }

.btn-icon { background: none; border: none; cursor: pointer; padding: 0.5rem; border-radius: 4px; color: #667eea; transition: all 0.2s; }

.btn-icon:hover { background: #f7fafc; }

.btn-icon.delete { color: #e53e3e; }

.btn-icon.delete:hover { background: #fff5f5; }

.empty-state { text-align: center; padding: 5rem 2rem; color: #718096; }

.empty-state svg { margin: 0 auto 1rem; color: #cbd5e0; }

.empty-state h3 { font-size: 1.5rem; color: #2d3748; margin-bottom: 0.5rem; }

/* Modal */
.modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center; z-index: 1000; padding: 1rem; }

.modal { background: white; border-radius: 16px; max-width: 600px; width: 100%; max-height: 90vh; overflow-y: auto; }

.modal-header { display: flex; justify-content: space-between; align-items: center; padding: 1.5rem; border-bottom: 1px solid #e2e8f0; }

.modal-header h3 { font-size: 1.5rem; color: #2d3748; margin: 0; }

.modal form { padding: 1.5rem; }

.form-group { margin-bottom: 1.5rem; }

.form-group label { display: block; font-weight: 600; margin-bottom: 0.5rem; color: #2d3748; }

.form-group input, .form-group textarea, .form-group select { width: 100%; padding: 0.75rem; border: 2px solid #e2e8f0; border-radius: 8px; font-size: 1rem; font-family: inherit; transition: all 0.2s; }

.form-group input:focus, .form-group textarea:focus, .form-group select:focus { outline: none; border-color: #667eea; box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1); }

.form-group input.error, .form-group textarea.error, .form-group select.error { border-color: #e53e3e; }

.form-row { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem; }

.error-message { color: #e53e3e; font-size: 0.875rem; margin-top: 0.25rem; display: block; }

.modal-actions { display: flex; gap: 1rem; justify-content: flex-end; padding-top: 1rem; border-top: 1px solid #e2e8f0; margin-top: 1rem; }

@media (max-width: 768px) {
  .tickets-grid { grid-template-columns: 1fr; }
  .tickets-header { flex-direction: column; align-items: stretch; }
  .tickets-header .btn { width: 100%; }
  .form-row { grid-template-columns: 1fr; }
  .modal { margin: 1rem; }
  .tickets-header h2 { font-size: 2rem; }
}
</style>