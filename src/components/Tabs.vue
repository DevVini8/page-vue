<template>
  <div class="tabs-container">
    <div class="tabs-header" role="tablist">
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        type="button"
        role="tab"
        class="tab-button"
        :class="{ active: activeTabIndex === index }"
        :aria-selected="activeTabIndex === index"
        @click="selectTab(index)"
      >
        {{ tab.title }}
      </button>
    </div>

    <div v-if="activeTab" class="tab-panel" role="tabpanel">
      <h4 class="tab-content-title">{{ activeTab.heading || activeTab.title }}</h4>
      <p class="tab-content-text">{{ activeTab.content }}</p>
      <ul v-if="activeTab.details && activeTab.details.length" class="tab-details-list">
        <li v-for="(detail, idx) in activeTab.details" :key="idx">
          <strong>{{ detail.label }}:</strong> {{ detail.value }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TabsComponent',
  props: {
    tabs: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      activeTabIndex: 0
    }
  },
  computed: {
    activeTab() {
      return this.tabs[this.activeTabIndex] || null
    }
  },
  methods: {
    selectTab(index) {
      this.activeTabIndex = index
      this.$emit('tab-change', this.tabs[index])
    }
  }
}
</script>

<style scoped>
.tabs-container {
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  background-color: #ffffff;
  overflow: hidden;
}

.tabs-header {
  display: flex;
  flex-wrap: wrap;
  background-color: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.tab-button {
  flex: 1 1 auto;
  padding: 0.875rem 1.25rem;
  border: none;
  background: transparent;
  font-size: 0.95rem;
  font-weight: 600;
  color: #64748b;
  cursor: pointer;
  border-bottom: 3px solid transparent;
  transition: all 0.2s ease;
}

.tab-button:hover {
  color: #1e293b;
  background-color: #f1f5f9;
}

.tab-button.active {
  color: #2563eb;
  background-color: #ffffff;
  border-bottom-color: #2563eb;
}

.tab-panel {
  padding: 1.5rem;
}

.tab-content-title {
  margin: 0 0 0.75rem 0;
  font-size: 1.15rem;
  color: #1e293b;
}

.tab-content-text {
  margin: 0 0 1rem 0;
  color: #475569;
  line-height: 1.6;
}

.tab-details-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 0.75rem;
}

.tab-details-list li {
  padding: 0.625rem 0.875rem;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 0.375rem;
  font-size: 0.9rem;
  color: #334155;
}
</style>
