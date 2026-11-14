<template>
  <div class="accordion">
    <div
      v-for="(item, index) in items"
      :key="index"
      class="accordion-item"
      :class="{ active: activeIndex === index }"
    >
      <button
        type="button"
        class="accordion-header"
        @click="toggleItem(index)"
        :aria-expanded="activeIndex === index"
      >
        <span class="accordion-title">{{ item.title }}</span>
        <span class="accordion-icon">{{ activeIndex === index ? '−' : '+' }}</span>
      </button>
      <div
        class="accordion-body-wrapper"
        :style="{ maxHeight: activeIndex === index ? '300px' : '0px' }"
      >
        <div class="accordion-body">
          <p>{{ item.content }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AccordionComponent',
  props: {
    items: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      activeIndex: 0
    }
  },
  methods: {
    toggleItem(index) {
      this.activeIndex = this.activeIndex === index ? null : index
      this.$emit('toggle', this.activeIndex)
    }
  }
}
</script>

<style scoped>
.accordion {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.accordion-item {
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  background-color: #ffffff;
  overflow: hidden;
  transition: border-color 0.25s ease, box-shadow 0.25s ease;
}

.accordion-item.active {
  border-color: #3b82f6;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.1);
}

.accordion-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.25rem;
  background: none;
  border: none;
  text-align: left;
  font-size: 1rem;
  font-weight: 600;
  color: #1e293b;
  cursor: pointer;
  transition: background-color 0.2s ease, color 0.2s ease;
}

.accordion-header:hover {
  background-color: #f8fafc;
}

.accordion-item.active .accordion-header {
  color: #2563eb;
  background-color: #eff6ff;
}

.accordion-icon {
  font-size: 1.25rem;
  font-weight: 700;
  line-height: 1;
  margin-left: 1rem;
}

.accordion-body-wrapper {
  overflow: hidden;
  transition: max-height 0.35s ease;
}

.accordion-body {
  padding: 1rem 1.25rem;
  color: #475569;
  line-height: 1.6;
  border-top: 1px solid #e2e8f0;
}
</style>
