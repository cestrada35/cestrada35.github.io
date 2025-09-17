<template>
  <div class="min-h-screen bg-gradient-to-b from-darker to-dark text-light font-outfit relative overflow-hidden">
    <!-- Floating Title Squrcle -->
    <div class="floating-squrcle title-squrcle">
      <div class="squrcle-inner">
        <span class="squrcle-text">Christian Estrada's Portfolio</span>
      </div>
    </div>

    <!-- Floating Profile Squrcle -->
    <div class="floating-squrcle profile-squrcle" @click="openAboutModal">
      <div class="squrcle-inner">
        <div class="profile-image-container">
          <img src="/profile_p.png" alt="Profile" class="profile-image" />
          <div class="ping-animation"></div>
          <div class="hover-tooltip">About Me</div>
        </div>
      </div>
    </div>

    <div class="container mx-auto px-4 py-12 max-w-6xl relative z-10">
      <header class="text-center mb-12 py-8 relative">
        <h1 class="text-4xl md:text-5xl font-bold text-lighter mb-4">Project Portfolio</h1>
        <p class="text-gray max-w-2xl mx-auto text-lg">A showcase of my latest work and personal projects</p>
        <div class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-20 h-1 bg-gradient-to-r from-primary to-secondary rounded-full"></div>
      </header>
      
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
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
      
      <AboutModal 
        :isOpen="aboutModalOpen" 
        @close="closeAboutModal" 
      />
    </div>
  </div>
</template>

<script>
import ProjectCard from './components/ProjectCard.vue'
import ProjectModal from './components/ProjectModal.vue'
import AboutModal from './components/AboutModal.vue'
import projectsData from './data/projects.json'

export default {
  name: 'App',
  components: {
    ProjectCard,
    ProjectModal,
    AboutModal
  },
  data() {
    return {
      projects: projectsData.projects,
      modalOpen: false,
      aboutModalOpen: false,
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
    },
    openAboutModal() {
      this.aboutModalOpen = true;
    },
    closeAboutModal() {
      this.aboutModalOpen = false;
    }
  }
}
</script>