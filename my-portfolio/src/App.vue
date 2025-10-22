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
        
        <!-- Modern Skills Toggle -->
        <div class="skills-toggle-section mb-16 mt-12">
          <div class="flex flex-col items-center">
            <!-- Toggle Button -->
            <button 
              class="skills-toggle-btn group"
              @click="toggleSkills"
              :class="{ 'skills-visible': skillsVisible }"
            >
              <div class="toggle-content">
                <span class="toggle-text">
                  {{ skillsVisible ? 'Hide Tech Stack' : 'View Tech Stack' }}
                </span>
                <div class="toggle-icon">
                  <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                    <path 
                      d="M4 6L8 10L12 6" 
                      stroke="currentColor" 
                      stroke-width="2" 
                      stroke-linecap="round" 
                      stroke-linejoin="round"
                      :class="{ 'rotate-180': skillsVisible }"
                    />
                  </svg>
                </div>
              </div>
              <div class="toggle-glow"></div>
            </button>

            <!-- Skills Grid -->
            <transition name="skills-slide">
              <div v-if="skillsVisible" class="skills-grid-container mt-8">
                <div class="skills-header mb-6 text-center">
                  <h3 class="text-xl font-semibold text-lighter mb-2">Technologies & Frameworks</h3>
                  <p class="text-gray text-sm">Some stats!</p>
                </div>
                
                <div class="skills-grid">
                  <div 
                    v-for="(skill, index) in uniqueSkills" 
                    :key="skill.name"
                    class="skill-card"
                    :class="`skill-tier-${skill.tier}`"
                    :style="getCardStyle(index)"
                    @mouseenter="hoveredSkill = skill.name"
                    @mouseleave="hoveredSkill = null"
                  >
                    <div class="skill-content">
                      <span class="skill-name">{{ skill.name }}</span>
                      <div class="skill-meta">
                        <span class="skill-count">{{ skill.count }} project{{ skill.count > 1 ? 's' : '' }}</span>
                      </div>
                    </div>
                    <div class="skill-progress">
                      <div 
                        class="progress-bar" 
                        :style="{ width: `${(skill.count / maxSkillCount) * 100}%` }"
                      ></div>
                    </div>
                  </div>
                </div>
              </div>
            </transition>
          </div>
        </div>
        <!-- End Modern Skills Toggle -->
        
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
      selectedProject: null,
      skillsVisible: false,
      hoveredSkill: null
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
    }
  },
  methods: {
    parallax() {
      const parallax = document.getElementById("parallax");
      if (parallax) {
        parallax.style.top = -(window.pageYOffset / 4) + 'px';
      }
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