<template>
  <div class="multiselect-box">
    <div class="filter-header">
      <h4 class="filter-title">Filter by Tech</h4>
      <button 
        v-if="selectedSkills.length > 0" 
        class="clear-btn" 
        @click="clearFilters"
      >
        Clear all
      </button>
    </div>

    <!-- The pool of clickable skill chips -->
    <div class="skills-pool">
      <span
        v-for="skill in allSkills"
        :key="skill"
        class="skill-chip"
        :class="{ 'chip-selected': isSelected(skill) }"
        @click="toggleSkill(skill)"
      >
        {{ skill }}
        <span class="chip-indicator">
          {{ isSelected(skill) ? '✓' : '+' }}
        </span>
      </span>
    </div>

    <!-- Status bar (helps the user know the filter is working) -->
    <div class="filter-status">
      <span v-if="selectedSkills.length === 0">
        Showing <strong>all</strong> projects
      </span>
      <span v-else>
        Showing <strong>{{ filteredProjectCount }}</strong> project(s) with 
        <strong>{{ selectedSkills.length }}</strong> selected skill(s)
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MultiselectBox',
  props: {
    // Full list of unique skill names (strings)
    allSkills: {
      type: Array,
      required: true
    },
    // Currently selected skills (from parent)
    selectedSkills: {
      type: Array,
      default: () => []
    },
    // We need the count to display "showing X projects"
    filteredProjectCount: {
      type: Number,
      default: 0
    }
  },
  emits: ['update:selectedSkills'],
  methods: {
    isSelected(skill) {
      return this.selectedSkills.includes(skill);
    },
    toggleSkill(skill) {
      const current = [...this.selectedSkills];
      const index = current.indexOf(skill);
      if (index > -1) {
        current.splice(index, 1);
      } else {
        current.push(skill);
      }
      this.$emit('update:selectedSkills', current);
    },
    clearFilters() {
      this.$emit('update:selectedSkills', []);
    }
  }
}
</script>

<style scoped>
.multiselect-box {
  background: rgba(115, 0, 255, 0.07);
  backdrop-filter: blur(8px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  padding: 24px 28px;
  margin-bottom: 32px;
  transition: all 0.3s ease;
}

.filter-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.filter-title {
  font-size: 1rem;
  font-weight: 600;
  color: #e0e0e0;
  margin: 0;
  letter-spacing: 0.3px;
}

.clear-btn {
  background: rgba(239, 68, 68, 0.15);
  color: #f87171;
  border: none;
  padding: 4px 14px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}
.clear-btn:hover {
  background: rgba(239, 68, 68, 0.3);
  transform: scale(1.04);
}

/* The chip container - this handles the responsive wrapping */
.skills-pool {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 12px;
  margin-bottom: 16px;
}

.skill-chip {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 16px 6px 18px;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 500;
  color: #a0aec0;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.06);
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.23, 1, 0.32, 1);
  user-select: none;
  letter-spacing: 0.2px;
}

.skill-chip:hover {
  transform: translateY(-3px);
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
  color: #ffffff;
}

/* Selected state */
.skill-chip.chip-selected {
  background: rgba(99, 102, 241, 0.25);
  border-color: #818cf8;
  color: #ffffff;
  box-shadow: 0 0 20px rgba(99, 102, 241, 0.15);
}
.skill-chip.chip-selected:hover {
  background: rgba(99, 102, 241, 0.35);
  box-shadow: 0 8px 25px rgba(99, 102, 241, 0.25);
}

.chip-indicator {
  font-size: 0.7rem;
  opacity: 0.6;
  transition: opacity 0.2s;
}
.chip-selected .chip-indicator {
  opacity: 1;
}

.filter-status {
  font-size: 0.8rem;
  color: #718096;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  padding-top: 14px;
  margin-top: 4px;
}
.filter-status strong {
  color: #cbd5e0;
  font-weight: 600;
}
</style>