<template>
  <transition name="modal">
    <div v-if="isOpen && project" class="modal-overlay" @click.self="close">
      <div class="modal-panel">
        <button class="modal-close" @click="close" aria-label="Close">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>

        <div class="cs">
          <div class="cs-eyebrow">{{ project.featuredLabel || 'Project' }}</div>
          <h2 class="cs-title">{{ project.title }}</h2>

          <div class="cs-tags">
            <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
          </div>

          <div v-if="project.image2" class="cs-plate">
            <img :src="project.image2" :alt="project.title" />
          </div>

          <template v-if="project.caseStudy">
            <section v-if="project.caseStudy.tip" class="cs-section">
              <h3>Tip:</h3>
              <p>{{ project.caseStudy.tip }}</p>
            </section>

            <section class="cs-section">
              <h3>The Problem</h3>
              <p>{{ project.caseStudy.problem }}</p>
            </section>

            <section class="cs-section">
              <h3>My Approach</h3>
              <p>{{ project.caseStudy.approach }}</p>
            </section>

            <section class="cs-section">
              <h3>The Hardest Bug</h3>
              <p>{{ project.caseStudy.hardestBug }}</p>
            </section>

            <section class="cs-section" v-if="project.caseStudy.adrs && project.caseStudy.adrs.length">
              <h3>Architecture Decisions</h3>
              <div class="cs-adrs">
                <div v-for="(adr, i) in project.caseStudy.adrs" :key="i" class="cs-adr">
                  <h4>{{ adr.title }}</h4>
                  <p>{{ adr.body }}</p>
                </div>
              </div>
            </section>

            <section class="cs-section">
              <h3>If I Had Two More Weeks</h3>
              <p>{{ project.caseStudy.reflection }}</p>
            </section>
          </template>

          <template v-else>
            <section class="cs-section">
              <p>{{ project.extendedDescription }}</p>
            </section>
            <section class="cs-section cs-pending">
              <p><em>A full case study for this project is in progress.</em></p>
            </section>
          </template>

          <div class="cs-links">
            <a
              v-if="project.link && project.link !== '#'"
              :href="project.link"
              target="_blank" rel="noopener"
              class="cs-link"
            >
              Live project <span aria-hidden="true">↗</span>
            </a>
            <a
              v-if="project.repo && project.repo !== '#'"
              :href="project.repo"
              target="_blank" rel="noopener"
              class="cs-link"
            >
              Source <span aria-hidden="true">↗</span>
            </a>
            <span
              v-if="!project.repo || project.repo === '#'"
              class="cs-private"
            >Private repository</span>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'CaseStudyModal',
  props: {
    isOpen: { type: Boolean, required: true },
    project: { type: Object, default: null }
  },
  emits: ['close'],
  watch: {
    isOpen(val) {
      document.body.style.overflow = val ? 'hidden' : ''
    }
  },
  methods: {
    close() {
      this.$emit('close')
    },
    onKey(e) {
      if (e.key === 'Escape' && this.isOpen) this.close()
    }
  },
  mounted() {
    window.addEventListener('keydown', this.onKey)
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.onKey)
    document.body.style.overflow = ''
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(20, 20, 20, 0.55);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: flex-start;
  justify-content: center;
  z-index: 1000;
  padding: 3rem 1.5rem;
  overflow-y: auto;
}

.modal-panel {
  background: var(--color-paper);
  border-radius: 4px;
  max-width: 760px;
  width: 100%;
  position: relative;
  box-shadow: 0 30px 60px -20px rgba(0, 0, 0, 0.35);
  border: 1px solid var(--color-rule);
  padding: 3.5rem 3rem;
  margin: auto 0;
}

.modal-close {
  position: absolute;
  top: 1.25rem; right: 1.25rem;
  background: transparent;
  border: 1px solid var(--color-rule);
  border-radius: 50%;
  width: 36px; height: 36px;
  display: flex; align-items: center; justify-content: center;
  color: var(--color-ink);
  transition: all 0.2s;
}
.modal-close:hover {
  background: var(--color-ink);
  color: var(--color-paper);
  border-color: var(--color-ink);
}

.cs-eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--color-accent);
  font-weight: 500;
  margin-bottom: 1rem;
}
.cs-title {
  font-size: clamp(2rem, 4vw, 2.75rem);
  font-weight: 300;
  letter-spacing: -0.02em;
  margin: 0 0 1.25rem;
  line-height: 1.05;
}
.cs-tags {
  display: flex; flex-wrap: wrap; gap: 0.4rem;
  margin-bottom: 2rem;
}
.cs-tags span {
  font-size: 0.72rem;
  padding: 0.25rem 0.7rem;
  border: 1px solid var(--color-rule);
  border-radius: 100px;
  color: var(--color-ink-soft);
}
.cs-plate {
  width: 100%;
  aspect-ratio: 16 / 9;
  border-radius: 4px;
  overflow: hidden;
  border: 1px solid var(--color-rule);
  margin-bottom: 2.5rem;
}
.cs-plate img { width: 100%; height: 100%; object-fit: cover; }

.cs-section {
  padding: 1.75rem 0;
  border-top: 1px solid var(--color-rule);
}
.cs-section h3 {
  font-size: 0.72rem;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--color-accent);
  font-family: var(--font-body);
  font-weight: 600;
  margin: 0 0 0.75rem;
}
.cs-section p {
  font-size: 1rem;
  color: var(--color-ink-soft);
  line-height: 1.75;
  margin: 0;
}
.cs-section p em {
  color: var(--color-ink-soft);
  font-style: italic;
}
.cs-pending {
  background: var(--color-paper-dark);
  padding: 1.5rem;
  border-radius: 4px;
  margin-top: 1rem;
  border-top: none;
}

.cs-adrs {
  display: flex; flex-direction: column; gap: 1.25rem;
}
.cs-adr h4 {
  font-family: var(--font-display);
  font-size: 1.05rem;
  font-weight: 500;
  margin: 0 0 0.3rem;
  color: var(--color-ink);
  letter-spacing: -0.01em;
}
.cs-adr p {
  font-size: 0.92rem;
  color: var(--color-ink-soft);
  margin: 0;
  line-height: 1.65;
}

.cs-links {
  display: flex;
  gap: 1.25rem;
  flex-wrap: wrap;
  padding-top: 2rem;
  border-top: 1px solid var(--color-rule);
  align-items: center;
}
.cs-link {
  color: var(--color-accent);
  font-weight: 500;
  font-size: 0.9rem;
  border-bottom: 1px solid var(--color-accent);
  padding-bottom: 2px;
  transition: opacity 0.2s;
}
.cs-link:hover { opacity: 0.75; }
.cs-private {
  font-size: 0.85rem;
  color: var(--color-ink-soft);
  font-style: italic;
}

/* transitions */
.modal-enter-active,
.modal-leave-active { transition: opacity 0.25s ease; }
.modal-enter-from,
.modal-leave-to { opacity: 0; }
.modal-enter-active .modal-panel,
.modal-leave-active .modal-panel { transition: transform 0.25s ease; }
.modal-enter-from .modal-panel,
.modal-leave-to .modal-panel { transform: translateY(12px); }

@media (max-width: 768px) {
  .modal-overlay { padding: 1rem; }
  .modal-panel { padding: 2.5rem 1.5rem; }
}
</style>