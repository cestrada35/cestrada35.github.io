<template>
  <div class="min-h-screen text-light font-outfit relative overflow-hidden">
    <!-- Parallax Background -->
    <div id="parallax" class="parallax-bg"></div>

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
        <h1 class="text-4xl md:text-5xl font-bold text-lighter mb-4">Christian Estrada</h1>
        <p class="text-gray max-w-2xl mx-auto text-lg">Hi, welcome to my portfolio, check out my work!</p>
        
        <div class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-20 h-1 bg-gradient-to-r from-primary to-secondary rounded-full"></div>
      </header>

      <!-- Multiselect Skills Box -->
        <MultiselectBox 
          :all-skills="skillNameList"
          :selected-skills="selectedSkills"
          :filtered-project-count="filteredProjects.length"
          @update:selected-skills="selectedSkills = $event"
        />
      <!-- End Multiselect Skills Box -->
      
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <ProjectCard 
          v-for="project in filteredProjects"
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
import MultiselectBox from './components/MultiselectBox.vue'
import projectsData from './data/projects.json'

export default {
  name: 'App',
  components: {
    ProjectCard,
    ProjectModal,
    MultiselectBox,
    AboutModal
  },
  data() {
    return {
      projects: projectsData.projects,
      modalOpen: false,
      aboutModalOpen: false,
      selectedProject: null,
      skillsVisible: false,
      hoveredSkill: null,
      selectedSkills: []
    }
  },
  computed: {
    uniqueSkills() {
      const allTags = this.projects.flatMap(project => project.tags);
      const skillCount = {};
      
      // Count occurrences of each skill
      allTags.forEach(skill => {
        skillCount[skill] = (skillCount[skill] || 0) + 1;
      });
      
      // Convert to array and assign tiers based on frequency
      return Object.entries(skillCount)
        .map(([name, count]) => {
          let tier = 1;
          if (count >= 3) tier = 3;      // Most used (prominent)
          else if (count >= 2) tier = 2; // Medium usage
          // else tier remains 1 (least used)
          
          return { name, count, tier };
        })
        .sort((a, b) => b.count - a.count); // Sort by frequency
    },
    maxSkillCount() {
      return Math.max(...this.uniqueSkills.map(skill => skill.count));
    },
    filteredProjects() {
    // If no filters are active, return all projects
    if (this.selectedSkills.length === 0) {
      return this.projects;
    }
    // Otherwise, filter by tag
    return this.projects.filter(project => {
      return project.tags.some(tag => this.selectedSkills.includes(tag));
    });
  },
  skillNameList() {
    return this.uniqueSkills.map(skill => skill.name);
  }
  },
  methods: {
    parallax() {
      const parallax = document.getElementById("parallax");
      if (!parallax) return;

      const viewportHeight = window.innerHeight;
      const imageHeight = parallax.offsetHeight;
      const container = parallax.parentElement;

      const containerRect = container.getBoundingClientRect();
      const containerTop = containerRect.top + window.scrollY;
      const containerHeight = containerRect.height;
      const scrollRange = containerHeight - viewportHeight;
      if (scrollRange <= 0) {
        parallax.style.top = '0px';
        return;
      }

      const scrolled = window.scrollY - containerTop;
      let progress = Math.min(1, Math.max(0, scrolled / scrollRange));
      const maxOffset = -(imageHeight - viewportHeight);
      const offset = progress * maxOffset;

      parallax.style.top = offset + 'px';
    },
    toggleSkills() {
      this.skillsVisible = !this.skillsVisible;
    },
    getCardStyle(index) {
      const delay = index * 0.05;
      return {
        animationDelay: `${delay}s`
      };
    },
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
  },
  mounted() {
    window.addEventListener("scroll", this.parallax, false);
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.parallax, false);
  }
}
</script>