<template>
  <transition name="modal">
    <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
      <div class="modal-container about-modal">
        <button class="modal-close" @click="closeModal">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
        
        <div class="modal-content">
          <div class="about-header">
            <div class="about-image-container">
              <img src="/profile_p.png" alt="Profile" class="about-image" />
            </div>
            <h2 class="about-title">About Me</h2>
          </div>
          
          <div class="about-details">
            <p class="about-description">
              Hello! I'm a passionate developer who loves creating beautiful, functional web experiences. 
              With expertise in modern JavaScript frameworks and a keen eye for design, I strive to build 
              applications that are both visually appealing and highly performant.
            </p>
            
            <div class="skills-section">
              <h3 class="skills-title">Skills & Technologies</h3>
              <div class="skills-grid">
                <span class="skill-tag">Vue.js</span>
                <span class="skill-tag">React</span>
                <span class="skill-tag">Node.js</span>
                <span class="skill-tag">Tailwind CSS</span>
                <span class="skill-tag">TypeScript</span>
                <span class="skill-tag">UI/UX Design</span>
              </div>
            </div>
            
            <div class="contact-section">
              <h3 class="contact-title">Get In Touch</h3>
              <div class="contact-links">
                <a href="mailto:your.email@example.com" class="contact-link">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path>
                    <polyline points="22,6 12,13 2,6"></polyline>
                  </svg>
                  Email
                </a>
                <a href="https://github.com/yourusername" target="_blank" class="contact-link">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
                  </svg>
                  GitHub
                </a>
                <a href="https://linkedin.com/in/yourusername" target="_blank" class="contact-link">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path>
                    <rect x="2" y="9" width="4" height="12"></rect>
                    <circle cx="4" cy="4" r="2"></circle>
                  </svg>
                  LinkedIn
                </a>
              </div>
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
    isOpen: {
      type: Boolean,
      required: true
    }
  },
  emits: ['close'],
  methods: {
    closeModal() {
      this.$emit('close');
    }
  },
  watch: {
    isOpen(newVal) {
      if (newVal) {
        document.body.style.overflow = 'hidden';
      } else {
        document.body.style.overflow = 'auto';
      }
    }
  }
}
</script>

<style scoped>
.about-modal {
  max-width: 600px;
}

.about-header {
  text-align: center;
  margin-bottom: 2rem;
}

.about-image-container {
  width: 100px;
  height: 100px;
  margin: 0 auto 1rem;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid var(--color-primary);
}

.about-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.about-title {
  font-size: 2rem;
  font-weight: 700;
  color: var(--color-lighter);
  margin-bottom: 0.5rem;
}

.about-description {
  color: var(--color-gray);
  line-height: 1.7;
  margin-bottom: 2rem;
  text-align: center;
}

.skills-section {
  margin-bottom: 2rem;
}

.skills-title {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--color-lighter);
  margin-bottom: 1rem;
  text-align: center;
}

.skills-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 0.5rem;
}

.skill-tag {
  background: rgba(139, 92, 246, 0.2);
  color: var(--color-primary);
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
}

.contact-section {
  text-align: center;
}

.contact-title {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--color-lighter);
  margin-bottom: 1rem;
}

.contact-links {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.contact-link {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  color: var(--color-primary);
  text-decoration: none;
  padding: 0.5rem 1rem;
  border: 1px solid rgba(139, 92, 246, 0.3);
  border-radius: 8px;
  transition: all 0.3s ease;
}

.contact-link:hover {
  background: rgba(139, 92, 246, 0.1);
  transform: translateY(-2px);
}

@media (max-width: 640px) {
  .contact-links {
    flex-direction: column;
    align-items: center;
  }
  
  .contact-link {
    width: 200px;
    justify-content: center;
  }
}
</style>