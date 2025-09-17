<template>
  <div class="min-h-screen bg-gradient-to-b from-darker to-dark text-light font-outfit">
    <div class="container py-12">
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
        />
      </div>
    </div>
  </div>
</template>

<script>
import ProjectCard from './components/ProjectCard.vue'
import projectsData from './data/projects.json'

export default {
  name: 'App',
  components: {
    ProjectCard
  },
  data() {
    return {
      projects: projectsData.projects
    }
  }
}
</script>