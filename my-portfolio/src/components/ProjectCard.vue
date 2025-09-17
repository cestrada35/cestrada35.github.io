<template>
  <div class="project-card slide-up" :class="{ visible: isVisible }" ref="card" @click="openModal">
    <div class="project-image">
      <div class="image-placeholder" :style="`background-image: url('${project.image}')`"></div>
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
      isVisible: false
    }
  },
  emits: ['open-modal'],
  methods: {
    openModal() {
      this.$emit('open-modal', this.project);
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
}

.project-card:hover {
  transform: translateY(-8px);
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
</style>