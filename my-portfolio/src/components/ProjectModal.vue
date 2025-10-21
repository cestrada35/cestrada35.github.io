<template>
  <transition name="modal">
    <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
      <div class="modal-container">
        <button class="modal-close" @click="closeModal">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
        
        <div class="modal-content">
          <div class="modal-image">
            <!-- <div v-if="project.image2" class="image-placeholder" :style="`background-image: url('${project.image}')`"></div> -->
            <div class="image-placeholder" :style="`background-image: url('${project.image2}')`"></div>
          </div>
          
          <div class="modal-details">
            <h2 class="modal-title">{{ project.title }}</h2>
            
            <div class="modal-tags">
              <span v-for="(tag, index) in project.tags" :key="index" class="tag">{{ tag }}</span>      
            </div>
            
            <p class="modal-description">{{ project.extendedDescription }}</p>
            <div v-if="project.repo">
              <a :href="project.link" class="project-link modal-link mr-4" target="_blank">
                View Project ↗
              </a>
              <a :href="project.repo" class="project-link modal-link" target="_blank">
                View Source Code ↗
              </a>
            </div>
            <div v-else>
              <p class="modal-link">Unfortunately this project belongs to a private repository!</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'ProjectModal',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    },
    project: {
      type: Object,
      default: () => ({})
    }
  },
  emits: ['close'],
  methods: {
    closeModal() {
      this.$emit('close');
    },
    startGif() {
      this.isGifPlaying = true;
    },
    stopGif() {
      this.isGifPlaying = false;
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
    .modal-overlay {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(2, 6, 23, 0.9);
        backdrop-filter: blur(5px);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 1000;
        padding: 1rem;
    }

    .modal-container {
        background: rgba(15, 23, 42, 0.95);
        border-radius: 16px;
        border: 1px solid rgba(139, 92, 246, 0.3);
        max-width: 800px;
        width: 100%;
        max-height: 90vh;
        overflow-y: auto;
        position: relative;
        box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
    }

    .modal-close {
        position: absolute;
        top: 1rem;
        right: 1rem;
        background: rgba(139, 92, 246, 0.2);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: var(--color-lighter);
        cursor: pointer;
        z-index: 10;
        transition: all 0.3s ease;
    }

    .modal-close:hover {
        background: rgba(139, 92, 246, 0.4);
        transform: rotate(90deg);
    }

    .modal-content {
        display: flex;
        flex-direction: column;
    }

    .modal-image {
        height: 400px;
        background: linear-gradient(45deg, var(--color-primary-dark), var(--color-primary));
        position: relative;
        overflow: hidden;
    }



    .modal-details {
        padding: 2rem;
    }

    .modal-title {
        font-size: 1.8rem;
        font-weight: 700;
        margin-bottom: 1rem;
        color: var(--color-lighter);
    }

    .modal-tags {
        display: flex;
        flex-wrap: wrap;
        gap: 0.5rem;
        margin-bottom: 1.5rem;
    }

    .modal-description {
        color: var(--color-gray);
        line-height: 1.7;
        margin-bottom: 2rem;
    }

    .modal-link {
        display: inline-flex;
        align-items: center;
        background: linear-gradient(to right, var(--color-primary), var(--color-secondary));
        color: white;
        padding: 0.75rem 1.5rem;
        border-radius: 8px;
        text-decoration: none;
        font-weight: 600;
        transition: all 0.3s ease;
    }

    .modal-link:hover {
        transform: translateY(-2px);
        box-shadow: 0 10px 25px -5px rgba(139, 92, 246, 0.4);
    }

    /* Modal transition animations */
    .modal-enter-active,
    .modal-leave-active {
        transition: opacity 0.3s ease;
    }

    .modal-enter-from,
    .modal-leave-to {
        opacity: 0;
    }

    .modal-enter-active .modal-container,
    .modal-leave-active .modal-container {
        transition: transform 0.3s ease, opacity 0.3s ease;
    }

    .modal-enter-from .modal-container,
    .modal-leave-to .modal-container {
        transform: scale(0.9);
        opacity: 0;
    }

    @media (max-width: 768px) {
        .modal-container {
            max-height: 95vh;
        }
        
        .modal-details {
            padding: 1.5rem;
        }
        
        .modal-title {
            font-size: 1.5rem;
        }

    }
</style>