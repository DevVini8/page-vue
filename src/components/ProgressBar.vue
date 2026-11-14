<template>
  <div class="progress-wrapper">
    <div class="progress-header">
      <span class="progress-label">{{ label }}</span>
      <span class="progress-percentage">{{ normalizedProgress }}%</span>
    </div>
    <div class="progress-track" role="progressbar" :aria-valuenow="normalizedProgress" aria-valuemin="0" aria-valuemax="100">
      <div
        class="progress-fill"
        :class="statusClass"
        :style="{ width: normalizedProgress + '%' }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProgressBarComponent',
  props: {
    progress: {
      type: Number,
      required: true,
      default: 0
    },
    label: {
      type: String,
      default: 'Progresso da Tarefa'
    }
  },
  computed: {
    normalizedProgress() {
      if (this.progress < 0) return 0
      if (this.progress > 100) return 100
      return Math.round(this.progress)
    },
    statusClass() {
      if (this.normalizedProgress === 100) return 'complete'
      if (this.normalizedProgress >= 60) return 'high'
      if (this.normalizedProgress >= 30) return 'medium'
      return 'low'
    }
  }
}
</script>

<style scoped>
.progress-wrapper {
  width: 100%;
}

.progress-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
  font-weight: 600;
  color: #334155;
}

.progress-percentage {
  color: #2563eb;
  font-weight: 700;
}

.progress-track {
  width: 100%;
  height: 1.25rem;
  background-color: #e2e8f0;
  border-radius: 9999px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  border-radius: 9999px;
  transition: width 0.35s ease, background-color 0.35s ease;
  background-color: #3b82f6;
}

.progress-fill.low {
  background-color: #f59e0b;
}

.progress-fill.medium {
  background-color: #3b82f6;
}

.progress-fill.high {
  background-color: #2563eb;
}

.progress-fill.complete {
  background-color: #10b981;
}
</style>
