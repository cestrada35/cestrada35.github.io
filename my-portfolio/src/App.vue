<template>
  <div class="app-container">
    <div class="container">
      <header class="header">
        <h1 class="title">Project Portfolio</h1>
        <p class="subtitle">A showcase of my latest work and personal projects</p>
        <div class="header-divider"></div>
      </header>
      
      <div class="projects-grid">
        <ProjectCard 
          v-for="project in projects" 
          :key="project.id" 
          :project="project" 
          @open-modal="openModal"
        />
      </div>
      
      <ProjectModal 
        :isOpen="modalOpen" 
        :project="selectedProject" 
        @close="closeModal" 
      />
    </div>
  </div>
</template>

<script>
import ProjectCard from './components/ProjectCard.vue'
import ProjectModal from './components/ProjectModal.vue'
import projectsData from './data/projects.json'

export default {
  name: 'App',
  components: {
    ProjectCard,
    ProjectModal
  },
  data() {
    return {
      projects: projectsData.projects,
      modalOpen: false,
      selectedProject: null
    }
  },
  methods: {
    openModal(project) {
      this.selectedProject = project;
      this.modalOpen = true;
    },
    closeModal() {
      this.modalOpen = false;
      this.selectedProject = null;
    }
  }
}
</script>

<!-- Keep the existing styles from previous step -->
<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(to bottom, var(--color-darker), var(--color-dark));
  color: var(--color-light);
  font-family: 'Outfit', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 3rem;
  padding: 2rem 0;
  position: relative;
}

.title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: var(--color-lighter);
}

.subtitle {
  font-size: 1.1rem;
  color: var(--color-gray);
  max-width: 600px;
  margin: 0 auto;
}

.header-divider {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 80px;
  height: 3px;
  background: linear-gradient(to right, var(--color-primary), var(--color-secondary));
  border-radius: 2px;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
  
  .container {
    padding: 1.5rem;
  }
  
  .title {
    font-size: 2rem;
  }
}
</style>