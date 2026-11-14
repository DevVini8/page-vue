<template>
  <div class="toast-container" aria-live="polite">
    <div
      v-for="toast in toasts"
      :key="toast.id"
      class="toast-item"
      :class="'toast-' + toast.type"
    >
      <div class="toast-content">
        <span class="toast-badge">{{ getTypeLabel(toast.type) }}</span>
        <span class="toast-message">{{ toast.message }}</span>
      </div>
      <button
        type="button"
        class="toast-close"
        @click="$emit('dismiss', toast.id)"
        aria-label="Fechar notificação"
      >
        ×
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ToastNotificationComponent',
  props: {
    toasts: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  methods: {
    getTypeLabel(type) {
      const labels = {
        success: 'Sucesso',
        error: 'Erro',
        info: 'Info'
      }
      return labels[type] || 'Aviso'
    }
  }
}
</script>

<style scoped>
.toast-container {
  position: fixed;
  top: 1.25rem;
  right: 1.25rem;
  z-index: 50;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  max-width: 360px;
  width: calc(100% - 2.5rem);
}

.toast-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.75rem;
  padding: 0.875rem 1rem;
  border-radius: 0.5rem;
  color: #ffffff;
  box-shadow: 0 10px 20px rgba(15, 23, 42, 0.18);
  animation: slideIn 0.25s ease;
}

.toast-success {
  background-color: #059669;
}

.toast-error {
  background-color: #dc2626;
}

.toast-info {
  background-color: #2563eb;
}

.toast-content {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.toast-badge {
  font-size: 0.7rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  opacity: 0.9;
}

.toast-message {
  font-size: 0.9rem;
  font-weight: 500;
  line-height: 1.4;
}

.toast-close {
  background: transparent;
  border: none;
  color: #ffffff;
  font-size: 1.35rem;
  line-height: 1;
  cursor: pointer;
  opacity: 0.85;
  padding: 0 0.25rem;
}

.toast-close:hover {
  opacity: 1;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
