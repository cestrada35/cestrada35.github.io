<template>
  <div class="site">
    <header class="site-header">
      <div class="wrap header-row">
        <div class="brand-group">
          <button class="brand-avatar" @click="openAboutModal" aria-label="About Christian">
            <img src="/profile_p.png" alt="" />
          </button>
          <a href="#" class="brand">Christian Estrada<span>.</span></a>
        </div>
        <nav class="site-nav">
          <a href="#work">Work</a>
          <button class="nav-link" @click="openAboutModal">About</button>
          <a href="#contact">Contact</a>
        </nav>
      </div>
    </header>

    <section class="hero">
      <div class="wrap">
        <div class="eyebrow">Software Developer — Glendale, CA</div>
        <h1>I build helpful, detail-oriented <em>software</em>.</h1>
        <p class="lede">
          A detail-oriented developer with a soft spot for <strong>architecture, functionality, and the small details</strong> that make an interface feel right. Formerly at UCLA's B.R.A.I.N. research center, where I shipped tools used by researchers and volunteers daily. Looking for my next challenge.
        </p>
        <div class="hero-meta">
          <div><span>Currently</span><b>Open to new roles</b></div>
          <div><span>Focus</span><b>Vue · Nuxt · TypeScript</b></div>
          <div><span>Based</span><b>Glendale, California</b></div>
        </div>
      </div>
    </section>

    <section class="filters" id="work">
      <div class="wrap">
        <SkillFilter
          :all-skills="allSkills"
          :selected-skills="selectedSkills"
          @update:selected-skills="selectedSkills = $event"
        />
      </div>
    </section>

    <section class="projects-section" v-if="filteredFeatured.length">
      <div class="wrap projects-list">
        <ProjectCard
          v-for="(project, i) in filteredFeatured"
          :key="project.id"
          :project="project"
          :index="i"
          variant="featured"
          :autoplay-gifs="true"
          @open-case-study="openCaseStudy"
        />
      </div>
    </section>

    <section class="other-section" v-if="filteredOthers.length">
      <div class="wrap">
        <div class="section-head">
          <h2>Also</h2>
          <span>{{ filteredOthers.length }} more</span>
        </div>
        <div class="other-grid">
          <ProjectCard
            v-for="project in filteredOthers"
            :key="project.id"
            :project="project"
            variant="compact"
            :autoplay-gifs="false"
            @open-case-study="openCaseStudy"
          />
        </div>
      </div>
    </section>

    <section class="empty-state" v-if="!filteredFeatured.length && !filteredOthers.length">
      <div class="wrap">
        <p>No projects match that filter. <button class="inline-link" @click="selectedSkills = []">Clear filters</button>.</p>
      </div>
    </section>

    <footer class="site-footer" id="contact">
      <div class="wrap footer-grid">
        <div>
          <h3>Let's <em>connect</em>?</h3>
          <p>I'm actively looking to connect with teams who need web development expertise. If this sounds like a good fit, I'd love to hear from you.</p>
          <a href="mailto:cestrada98@outlook.com" class="btn-primary">Get in touch</a>
        </div>
        <div class="footer-meta">
          <a href="https://github.com/cestrada35" target="_blank" rel="noopener">GitHub ↗</a>
          <a href="https://www.linkedin.com/in/christian-estrada-dev/" target="_blank" rel="noopener">LinkedIn ↗</a>
          <a href="https://cestrada35.github.io/" target="_blank" rel="noopener">Resume ↗</a>
          <div class="footer-copy">© 2026 Christian Estrada</div>
        </div>
      </div>
    </footer>

    <CaseStudyModal
      :is-open="caseStudyOpen"
      :project="selectedProject"
      @close="closeCaseStudy"
    />
    <AboutModal
      :is-open="aboutModalOpen"
      @close="closeAboutModal"
    />
  </div>
</template>

<script>
import ProjectCard from './components/ProjectCard.vue'
import CaseStudyModal from './components/CaseStudyModal.vue'
import AboutModal from './components/AboutModal.vue'
import SkillFilter from './components/SkillFilter.vue'
import projectsData from './data/projects.json'

export default {
  name: 'App',
  components: {
    ProjectCard,
    CaseStudyModal,
    AboutModal,
    SkillFilter
  },
  data() {
    return {
      projects: projectsData.projects,
      selectedSkills: [],
      caseStudyOpen: false,
      selectedProject: null,
      aboutModalOpen: false
    }
  },
  computed: {
    allSkills() {
      const all = this.projects.flatMap(p => p.tags)
      return [...new Set(all)].sort()
    },
    filteredProjects() {
      if (this.selectedSkills.length === 0) return this.projects
      return this.projects.filter(p =>
        p.tags.some(tag => this.selectedSkills.includes(tag))
      )
    },
    filteredFeatured() {
      return this.filteredProjects
        .filter(p => p.featured)
        .sort((a, b) => (a.featuredOrder || 99) - (b.featuredOrder || 99))
    },
    filteredOthers() {
      return this.filteredProjects.filter(p => !p.featured)
    }
  },
  methods: {
    openCaseStudy(project) {
      this.selectedProject = project
      this.caseStudyOpen = true
    },
    closeCaseStudy() {
      this.caseStudyOpen = false
      this.selectedProject = null
    },
    openAboutModal() {
      this.aboutModalOpen = true
    },
    closeAboutModal() {
      this.aboutModalOpen = false
    }
  }
}
</script>

<style scoped>

.brand-group {
  display: flex;
  align-items: center;
  gap: 0.85rem;
}
.brand-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  overflow: hidden;
  border: 1px solid var(--color-rule);
  background: var(--color-paper-dark);
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: all 0.25s ease;
}
.brand-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.brand-avatar:hover {
  border-color: var(--color-accent);
  transform: scale(1.06);
  box-shadow: 0 4px 14px rgba(168, 62, 44, 0.18);
}
.brand-avatar:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
}

@media (max-width: 768px) {
  .brand-avatar { width: 34px; height: 34px; }
  .brand-group { gap: 0.65rem; }
}

.site { min-height: 100vh; display: flex; flex-direction: column; }

/* Header */
.site-header {
  padding: 2rem 0;
  border-bottom: 1px solid var(--color-rule);
  position: sticky; top: 0;
  background: rgba(246, 243, 238, 0.92);
  backdrop-filter: blur(10px);
  z-index: 20;
}
.header-row {
  display: flex; justify-content: space-between; align-items: center;
  gap: 2rem;
}
.brand {
  font-family: var(--font-display);
  font-weight: 600;
  font-size: 1.15rem;
  letter-spacing: -0.01em;
}
.brand span { color: var(--color-accent); }
.site-nav { display: flex; align-items: center; gap: 2rem; }
.site-nav a, .site-nav .nav-link {
  color: var(--color-ink-soft);
  background: transparent; border: none; padding: 0;
  font-size: 0.85rem; font-weight: 500;
  letter-spacing: 0.02em;
  transition: color 0.2s;
}
.site-nav a:hover, .site-nav .nav-link:hover { color: var(--color-accent); }

/* Hero */
.hero { padding: 6rem 0 5rem; border-bottom: 1px solid var(--color-rule); }
.eyebrow {
  font-size: 0.72rem; font-weight: 500;
  letter-spacing: 0.18em; text-transform: uppercase;
  color: var(--color-accent); margin-bottom: 2rem;
  display: flex; align-items: center; gap: 1rem;
}
.eyebrow::after {
  content: ''; flex: 1; height: 1px; background: var(--color-rule);
}
.hero h1 {
  font-size: clamp(2.8rem, 6vw, 5rem);
  font-weight: 300;
  line-height: 1.02;
  letter-spacing: -0.03em;
  margin: 0 0 2rem;
  max-width: 900px;
}
.hero h1 em {
  font-style: italic;
  font-weight: 400;
  color: var(--color-accent);
}
.lede {
  font-size: 1.1rem;
  max-width: 620px;
  color: var(--color-ink-soft);
  line-height: 1.7;
  margin: 0 0 3rem;
}
.lede strong { color: var(--color-ink); font-weight: 500; }
.hero-meta {
  display: flex; gap: 3rem; flex-wrap: wrap;
  font-size: 0.85rem; color: var(--color-ink-soft);
}
.hero-meta div span {
  display: block; font-size: 0.68rem;
  letter-spacing: 0.15em; text-transform: uppercase;
  color: var(--color-accent); margin-bottom: 0.3rem;
  font-weight: 500;
}
.hero-meta div b { color: var(--color-ink); font-weight: 600; }

/* Filters */
.filters { padding: 2.5rem 0; border-bottom: 1px solid var(--color-rule); }

/* Featured project list */
.projects-section { padding: 2rem 0 1rem; }
.projects-list { padding-top: 1rem; }

/* Other projects */
.other-section { padding: 4rem 0; }
.section-head {
  display: flex; justify-content: space-between; align-items: baseline;
  margin-bottom: 2.5rem;
  padding-bottom: 1.25rem;
  border-bottom: 1px solid var(--color-rule);
}
.section-head h2 {
  font-size: 2rem; font-weight: 400;
  letter-spacing: -0.01em; margin: 0;
}
.section-head span {
  font-size: 0.8rem; color: var(--color-ink-soft);
  font-weight: 500;
}
.other-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem 2.5rem;
}

/* Empty state */
.empty-state { padding: 5rem 0; }
.empty-state p {
  color: var(--color-ink-soft); font-size: 0.95rem;
  text-align: center;
}
.inline-link {
  background: none; border: none; padding: 0;
  color: var(--color-accent); font-weight: 500;
  text-decoration: underline; text-underline-offset: 3px;
}

/* Footer */
.site-footer {
  padding: 5rem 0 3rem;
  border-top: 1px solid var(--color-rule);
  margin-top: auto;
}
.footer-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: end;
}
.site-footer h3 {
  font-size: clamp(2rem, 4vw, 3rem);
  font-weight: 300;
  line-height: 1.1;
  letter-spacing: -0.02em;
  margin: 0 0 1.5rem;
}
.site-footer h3 em {
  font-style: italic;
  color: var(--color-accent);
}
.site-footer p {
  color: var(--color-ink-soft);
  max-width: 420px;
  margin: 0 0 2rem;
}
.btn-primary {
  display: inline-flex; align-items: center; gap: 0.75rem;
  background: var(--color-ink); color: var(--color-paper);
  padding: 1rem 2rem; border-radius: 100px;
  font-size: 0.9rem; font-weight: 500;
  text-decoration: none;
  transition: all 0.2s;
}
.btn-primary:hover {
  background: var(--color-accent);
  transform: translateY(-2px);
}
.footer-meta {
  text-align: right; font-size: 0.85rem; color: var(--color-ink-soft);
  display: flex; flex-direction: column; gap: 0.4rem;
}
.footer-meta a {
  color: var(--color-ink);
  transition: color 0.2s;
  font-weight: 500;
}
.footer-meta a:hover { color: var(--color-accent); }
.footer-copy {
  margin-top: 2rem;
  font-size: 0.78rem;
  color: var(--color-ink-soft);
}

@media (max-width: 900px) {
  .other-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 768px) {
  .hero { padding: 4rem 0 3rem; }
  .other-grid { grid-template-columns: 1fr; gap: 2rem; }
  .footer-grid { grid-template-columns: 1fr; gap: 2rem; }
  .footer-meta { text-align: left; }
  .site-nav { gap: 1.25rem; }
  .hero-meta { gap: 1.5rem; }
}
</style>