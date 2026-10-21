<template>
  <transition name="modal">
    <div v-if="isOpen" class="modal-overlay" @click.self="close">
      <div class="modal-panel">
        <button class="modal-close" @click="close" aria-label="Close">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>

        <div class="about">
          <div class="about-eyebrow">About</div>

          <div class="about-head">
            <div class="about-image">
              <img src="/profile_p.png" alt="Christian Estrada" />
            </div>
            <div>
              <h2 class="about-title">Hi, I'm Christian.</h2>
              <div class="about-meta">Full-stack developer · Glendale, CA</div>
            </div>
          </div>

          <p class="about-body">
            I'm a full-stack web developer based in Glendale, CA. I specialize in turning teams' ideas into clean, performant web applications. With a strong emphasis on maintainable architecture, thoughtful design, and the belief that great software is built collaboratively. I'm confident in my ability to contribute to your team's success, and I'd love to discuss a potential collaboration.
          </p>

          <div class="about-contact">
            <h3 class="about-contact-title">Let's get in touch</h3>
            <div class="contact-links">
              <a href="mailto:cestrada98@outlook.com" class="contact-link">
                Email
              </a>
              <a href="https://github.com/cestrada35" target="_blank" rel="noopener" class="contact-link">
                GitHub
              </a>
              <a href="https://www.linkedin.com/in/christian-estrada-dev/" target="_blank" rel="noopener" class="contact-link">
                LinkedIn
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'AboutModal',
  props: {
    isOpen: { type: Boolean, required: true }
  },
  emits: ['close'],
  watch: {
    isOpen(val) {
      document.body.style.overflow = val ? 'hidden' : ''
    }
  },
  methods: {
    close() { this.$emit('close') },
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
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1.5rem;
}

.modal-panel {
  background: var(--color-paper);
  border-radius: 4px;
  max-width: 620px;
  width: 100%;
  position: relative;
  box-shadow: 0 30px 60px -20px rgba(0, 0, 0, 0.35);
  border: 1px solid var(--color-rule);
  padding: 3rem 2.5rem;
  max-height: 90vh;
  overflow-y: auto;
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

.about-eyebrow {
  font-size: 0.72rem;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--color-accent);
  font-weight: 500;
  margin-bottom: 1.5rem;
}

.about-head {
  display: flex; gap: 1.5rem; align-items: center;
  margin-bottom: 2rem;
}
.about-image {
  width: 80px; height: 80px;
  border-radius: 50%;
  overflow: hidden;
  border: 1px solid var(--color-rule);
  flex-shrink: 0;
}
.about-image img { width: 100%; height: 100%; object-fit: cover; }

.about-title {
  font-family: var(--font-display);
  font-size: 1.85rem;
  font-weight: 400;
  letter-spacing: -0.01em;
  margin: 0 0 0.4rem;
}
.about-meta {
  font-size: 0.78rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--color-ink-soft);
  font-weight: 500;
}

.about-body {
  color: var(--color-ink-soft);
  font-size: 1rem;
  line-height: 1.75;
  margin: 0 0 2.5rem;
}

.about-contact {
  border-top: 1px solid var(--color-rule);
  padding-top: 2rem;
}
.about-contact-title {
  font-family: var(--font-display);
  font-size: 1.3rem;
  font-weight: 400;
  margin: 0 0 1rem;
  color: var(--color-ink);
}
.contact-links {
  display: flex; gap: 0.75rem; flex-wrap: wrap;
}
.contact-link {
  padding: 0.65rem 1.25rem;
  border: 1px solid var(--color-rule);
  border-radius: 100px;
  color: var(--color-ink);
  font-size: 0.85rem;
  font-weight: 500;
  transition: all 0.2s;
}
.contact-link:hover {
  background: var(--color-ink);
  border-color: var(--color-ink);
  color: var(--color-paper);
}

.modal-enter-active,
.modal-leave-active { transition: opacity 0.25s ease; }
.modal-enter-from,
.modal-leave-to { opacity: 0; }
.modal-enter-active .modal-panel,
.modal-leave-active .modal-panel { transition: transform 0.25s ease; }
.modal-enter-from .modal-panel,
.modal-leave-to .modal-panel { transform: translateY(12px); }

@media (max-width: 640px) {
  .modal-panel { padding: 2.5rem 1.5rem; }
  .about-head { flex-direction: column; align-items: flex-start; gap: 1rem; }
}
</style>