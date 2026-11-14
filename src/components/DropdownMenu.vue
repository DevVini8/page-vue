<template>
  <div class="dropdown" ref="dropdownRef">
    <button
      type="button"
      class="dropdown-trigger"
      @click="toggleMenu"
      :aria-expanded="isOpen"
    >
      <span>{{ selectedLabel }}</span>
      <span class="dropdown-arrow" :class="{ open: isOpen }">▼</span>
    </button>

    <ul v-if="isOpen" class="dropdown-menu">
      <li
        v-for="(option, index) in options"
        :key="index"
        class="dropdown-item"
        :class="{ selected: selectedOption && selectedOption.value === option.value }"
        @click="selectOption(option)"
      >
        {{ option.label }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'DropdownMenuComponent',
  props: {
    label: {
      type: String,
      default: 'Selecione uma opção'
    },
    options: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      isOpen: false,
      selectedOption: null
    }
  },
  computed: {
    selectedLabel() {
      return this.selectedOption ? this.selectedOption.label : this.label
    }
  },
  methods: {
    toggleMenu() {
      this.isOpen = !this.isOpen
    },
    selectOption(option) {
      this.selectedOption = option
      this.isOpen = false
      this.$emit('select', option)
    },
    handleClickOutside(event) {
      if (this.$refs.dropdownRef && !this.$refs.dropdownRef.contains(event.target)) {
        this.isOpen = false
      }
    }
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside)
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside)
  }
}
</script>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
  width: 100%;
  max-width: 280px;
}

.dropdown-trigger {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 0.5rem;
  font-size: 0.95rem;
  font-weight: 500;
  color: #1e293b;
  cursor: pointer;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.dropdown-trigger:hover {
  border-color: #94a3b8;
}

.dropdown-trigger:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
}

.dropdown-arrow {
  font-size: 0.75rem;
  color: #64748b;
  transition: transform 0.2s ease;
}

.dropdown-arrow.open {
  transform: rotate(180deg);
}

.dropdown-menu {
  position: absolute;
  top: calc(100% + 0.375rem);
  left: 0;
  right: 0;
  z-index: 30;
  margin: 0;
  padding: 0.375rem 0;
  list-style: none;
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  box-shadow: 0 10px 25px rgba(15, 23, 42, 0.12);
}

.dropdown-item {
  padding: 0.625rem 1rem;
  font-size: 0.925rem;
  color: #334155;
  cursor: pointer;
  transition: background-color 0.15s ease, color 0.15s ease;
}

.dropdown-item:hover {
  background-color: #eff6ff;
  color: #2563eb;
}

.dropdown-item.selected {
  background-color: #dbeafe;
  color: #1d4ed8;
  font-weight: 600;
}
</style>
