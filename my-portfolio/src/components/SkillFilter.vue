<template>
  <div class="filter">
    <div class="filter-label">Filter by stack</div>
    <div class="chips">
      <button
        class="chip"
        :class="{ active: selectedSkills.length === 0 }"
        @click="clear"
      >
        All
      </button>
      <button
        v-for="skill in allSkills"
        :key="skill"
        class="chip"
        :class="{ active: selectedSkills.includes(skill) }"
        @click="toggle(skill)"
      >
        {{ skill }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SkillFilter',
  props: {
    allSkills: { type: Array, required: true },
    selectedSkills: { type: Array, default: () => [] }
  },
  emits: ['update:selected-skills'],
  methods: {
    toggle(skill) {
      const next = [...this.selectedSkills]
      const i = next.indexOf(skill)
      if (i > -1) next.splice(i, 1)
      else next.push(skill)
      this.$emit('update:selected-skills', next)
    },
    clear() {
      this.$emit('update:selected-skills', [])
    }
  }
}
</script>

<style scoped>
.filter-label {
  font-size: 0.7rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--color-ink-soft);
  margin-bottom: 1rem;
  font-weight: 500;
}
.chips {
  display: flex; flex-wrap: wrap; gap: 0.5rem;
}
.chip {
  padding: 0.45rem 1rem;
  border: 1px solid var(--color-rule);
  border-radius: 100px;
  background: transparent;
  color: var(--color-ink-soft);
  font-size: 0.82rem;
  font-family: inherit;
  transition: all 0.2s;
}
.chip:hover {
  border-color: var(--color-accent);
  color: var(--color-accent);
}
.chip.active {
  background: var(--color-accent);
  color: var(--color-paper);
  border-color: var(--color-accent);
}
</style>