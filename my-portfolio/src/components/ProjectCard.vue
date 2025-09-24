<template>
  <div class="project-card slide-up" :class="{ visible: isVisible }" ref="card" @click="openModal">
    <div class="project-image">
      <!-- Show GIF on hover, static image by default -->
      <div class="image-container">
        <img 
          v-if="project.gif" 
          :src="project.gif" 
          alt="Project demo" 
          class="project-gif"
          @mouseenter="startGif"
          @mouseleave="stopGif"
        />
        <div v-else class="image-placeholder" :style="`background-image: url('${project.image}')`"></div>
      </div>
    </div>
    <div class="project-content">
      <h3 class="project-title">{{ project.title }}</h3>
      <p class="project-description">{{ project.description }}</p>
      <div class="project-tags">
        <span v-for="(tag, index) in project.tags" :key="index" class="tag">{{ tag }}</span>
      </div>
      <div class="view-project-hint">Click to view details →</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProjectCard',
  props: {
    project: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isVisible: false,
      isGifPlaying: false
    }
  },
  emits: ['open-modal'],
  methods: {
    openModal() {
      this.$emit('open-modal', this.project);
    },
    startGif() {
        console.log('Starting gif')
        this.isGifPlaying = true;
    },
    stopGif() {
        this.isGifPlaying = false;
    }
  },
  mounted() {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.isVisible = true;
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1 });
    
    observer.observe(this.$refs.card);
  }
}
</script>

<style scoped>
.project-card {
  cursor: pointer;
  transition: all 0.3s ease;
  overflow: hidden; /* Important for GIF container */
}

.project-card:hover {
  transform: translateY(-8px);
}

.image-container {
  position: relative;
  width: 100%;
  height: 200px; /* Adjust based on your design */
  overflow: hidden;
  border-radius: 8px;
}

.project-gif {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.project-card:hover .project-gif {
  transform: scale(1.05);
}

.image-placeholder {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.view-project-hint {
  color: var(--color-primary);
  font-size: 0.9rem;
  font-weight: 500;
  margin-top: 1rem;
  opacity: 0.8;
  transition: opacity 0.3s ease;
}

.project-card:hover .view-project-hint {
  opacity: 1;
}

/* Optional: Add a play icon overlay for GIFs */
.image-container::after {
  content: '▶';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.project-card:hover .image-container::after {
  opacity: 1;
}
</style>