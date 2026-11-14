<template>
  <nav class="pagination" aria-label="Navegação de páginas">
    <button
      type="button"
      class="page-btn nav-btn"
      :disabled="currentPage <= 1"
      @click="changePage(currentPage - 1)"
    >
      Anterior
    </button>

    <div class="page-numbers">
      <button
        v-for="page in pages"
        :key="page"
        type="button"
        class="page-btn"
        :class="{ active: page === currentPage }"
        @click="changePage(page)"
      >
        {{ page }}
      </button>
    </div>

    <button
      type="button"
      class="page-btn nav-btn"
      :disabled="currentPage >= totalPages"
      @click="changePage(currentPage + 1)"
    >
      Próxima
    </button>
  </nav>
</template>

<script>
export default {
  name: 'PaginationComponent',
  props: {
    totalPages: {
      type: Number,
      required: true,
      default: 1
    },
    currentPage: {
      type: Number,
      required: true,
      default: 1
    }
  },
  computed: {
    pages() {
      const list = []
      for (let i = 1; i <= this.totalPages; i++) {
        list.push(i)
      }
      return list
    }
  },
  methods: {
    changePage(page) {
      if (page >= 1 && page <= this.totalPages && page !== this.currentPage) {
        this.$emit('change-page', page)
      }
    }
  }
}
</script>

<style scoped>
.pagination {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 1.25rem;
}

.page-numbers {
  display: flex;
  flex-wrap: wrap;
  gap: 0.375rem;
}

.page-btn {
  min-width: 2.5rem;
  height: 2.5rem;
  padding: 0 0.75rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.375rem;
  background-color: #ffffff;
  color: #334155;
  font-size: 0.875rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.page-btn:hover:not(:disabled) {
  background-color: #f1f5f9;
  border-color: #94a3b8;
}

.page-btn.active {
  background-color: #2563eb;
  border-color: #2563eb;
  color: #ffffff;
}

.page-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  background-color: #f8fafc;
}

.nav-btn {
  padding: 0 1rem;
}
</style>
