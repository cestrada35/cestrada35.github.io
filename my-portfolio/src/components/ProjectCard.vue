<template>
  <div class="project-card slide-up" :class="{ visible: isVisible }" ref="card">
    <div class="project-image">
      <div class="image-placeholder" :style="`background-image: url('${project.image}')`"></div>
    </div>
    <div class="project-content">
      <h3 class="project-title">{{ project.title }}</h3>
      <p class="project-description">{{ project.description }}</p>
      <div class="project-tags">
        <span v-for="(tag, index) in project.tags" :key="index" class="tag">{{ tag }}</span>
      </div>
      <a :href="project.link" class="project-link">View Project →</a>
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
.project-image {
  height: 180px;
  background: linear-gradient(45deg, var(--color-primary-dark), var(--color-primary));
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.image-placeholder {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0.3;
  background-size: cover;
}

.project-content {
  padding: 1.5rem;
}

.project-title {
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-lighter);
}

.project-description {
  color: var(--color-gray);
  font-size: 0.95rem;
  margin-bottom: 1.5rem;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.project-link {
  display: inline-flex;
  align-items: center;
  color: var(--color-primary);
  text-decoration: none;
  font-weight: 500;
  transition: all 0.3s ease;
}

.project-link:hover {
  color: var(--color-secondary);
}
</style>