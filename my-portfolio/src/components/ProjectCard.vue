<template>
  <!-- Featured variant: numbered editorial row -->
  <article v-if="variant === 'featured'" class="project project--featured">
    <div class="project-num">{{ paddedIndex }}</div>

    <div class="project-left">
      <div class="project-meta">{{ project.featuredLabel || 'Featured' }}</div>
      <h2 class="project-title">{{ project.title }}</h2>
      <p class="project-desc">{{ project.description }}</p>
    </div>

    <div class="project-right">
      <button
        class="plate plate--featured"
        @mouseenter="onEnter"
        @mouseleave="onLeave"
        @click="$emit('open-case-study', project)"
        :aria-label="`View ${project.title}`"
      >
        <!-- Blurred backdrop -->
        <div v-if="backdropSrc && !imgFailed" class="plate-bg" aria-hidden="true">
          <img :src="backdropSrc" class="plate-backdrop" loading="lazy" alt="" @error="imgFailed = true" />
          <div class="plate-tint"></div>
        </div>

        <!-- Frame: base + hover overlay stacked in one grid cell -->
        <div class="plate-frame">
          <!-- Base: GIF if autoplay, else static image, else fallback -->
          <img
            v-if="autoplayGifs && project.gif && !gifFailed"
            :src="project.gif"
            :alt="`${project.title} demo`"
            class="plate-media"
            :style="gifStyle"
            loading="lazy"
            @error="gifFailed = true"
          />
          <img
            v-else-if="backdropSrc && !imgFailed"
            :src="backdropSrc"
            :alt="project.title"
            class="plate-media"
            loading="lazy"
            @error="imgFailed = true"
          />
          <div v-else class="plate-fallback">{{ initials }}</div>

          <!-- Hover overlay GIF: only when not autoplaying -->
          <img
            v-if="!autoplayGifs && project.gif && hasHovered && !gifFailed"
            :src="project.gif"
            :alt="`${project.title} demo`"
            class="plate-media plate-gif"
            :class="{ visible: hovered }"
            :style="gifStyle"
            @error="gifFailed = true"
          />
        </div>
      </button>

      <div class="project-tags">
        <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
      </div>

      <button class="project-link" @click="$emit('open-case-study', project)">
        {{ project.caseStudy ? 'Read case study' : 'View project' }}
        <span aria-hidden="true">→</span>
      </button>
    </div>
  </article>

  <!-- Compact variant: small tile for "Also" section -->
  <article v-else class="project project--compact">
    <button
      class="plate plate--compact"
      @mouseenter="onEnter"
      @mouseleave="onLeave"
      @click="$emit('open-case-study', project)"
      :aria-label="`View ${project.title}`"
    >
      <div v-if="backdropSrc && !imgFailed" class="plate-bg" aria-hidden="true">
        <img :src="backdropSrc" class="plate-backdrop" loading="lazy" alt="" @error="imgFailed = true" />
        <div class="plate-tint"></div>
      </div>

      <div class="plate-frame">
        <img
          v-if="backdropSrc && !imgFailed"
          :src="backdropSrc"
          :alt="project.title"
          class="plate-media"
          loading="lazy"
          @error="imgFailed = true"
        />
        <div v-else class="plate-fallback">{{ initials }}</div>

        <!-- Hover GIF for compact -->
        <img
          v-if="project.gif && hasHovered && !gifFailed"
          :src="project.gif"
          :alt="`${project.title} demo`"
          class="plate-media plate-gif"
          :class="{ visible: hovered }"
          :style="gifStyle"
          @error="gifFailed = true"
        />
      </div>
    </button>

    <div class="compact-body">
      <h3 class="compact-title">{{ project.title }}</h3>
      <p class="compact-desc">{{ project.description }}</p>
      <div class="compact-tags">
        <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  name: 'ProjectCard',
  props: {
    project: { type: Object, required: true },
    index: { type: Number, default: 0 },
    variant: { type: String, default: 'featured' },
    autoplayGifs: { type: Boolean, default: true }
  },
  emits: ['open-case-study'],
  data() {
    return {
      hovered: false,
      hasHovered: false,
      gifFailed: false,
      imgFailed: false
    }
  },
  computed: {
    paddedIndex() {
      return String(this.index + 1).padStart(2, '0')
    },
    initials() {
      return this.project.title
        .split(' ')
        .map(w => w[0])
        .join('')
        .slice(0, 2)
        .toUpperCase()
    },
    backdropSrc() {
      return this.project.image2 || this.project.image || ''
    },
    gifStyle() {
      const offset = this.project.gif_offset
      if (typeof offset === 'number') {
        return { objectPosition: `0 ${offset}px` }
      }
      return {}
    }
  },
  methods: {
    onEnter() {
      this.hovered = true
      this.hasHovered = true
    },
    onLeave() {
      this.hovered = false
    }
  }
}
</script>

<style scoped>
/* ---------- Featured layout ---------- */
.project--featured {
  display: grid;
  grid-template-columns: 80px 1fr 1fr;
  gap: 3rem;
  align-items: start;
  padding: 3.5rem 0;
  border-bottom: 1px solid var(--color-rule);
  transition: background 0.3s;
  position: relative;
}
.project--featured:hover { background: var(--color-paper-dark); }

.project-num {
  font-family: var(--font-display);
  font-size: 2.5rem;
  font-weight: 300;
  color: var(--color-accent);
  line-height: 1;
}

.project-meta {
  font-size: 0.72rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-ink-soft);
  margin-bottom: 0.75rem;
  font-weight: 500;
}

.project-title {
  font-family: var(--font-display);
  font-size: 1.9rem;
  font-weight: 400;
  line-height: 1.15;
  letter-spacing: -0.02em;
  margin: 0 0 0.75rem;
  color: var(--color-ink);
}

.project-desc {
  color: var(--color-ink-soft);
  font-size: 0.95rem;
  max-width: 420px;
  margin: 0;
}

.project-right { padding-top: 2rem; }

.project-tags {
  display: flex; flex-wrap: wrap; gap: 0.4rem;
  margin-bottom: 1.25rem;
}
.project-tags span {
  font-size: 0.72rem;
  padding: 0.25rem 0.7rem;
  border: 1px solid var(--color-rule);
  border-radius: 100px;
  color: var(--color-ink-soft);
}

.project-link {
  display: inline-flex; align-items: center; gap: 0.5rem;
  background: none; border: none; padding: 0;
  color: var(--color-accent);
  font-size: 0.85rem; font-weight: 500;
  border-bottom: 1px solid var(--color-accent);
  padding-bottom: 2px;
  transition: gap 0.2s;
}
.project-link:hover { gap: 0.8rem; }

/* ---------- The plate (shared by featured + compact) ---------- */
.plate {
  display: block;
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 10;
  border-radius: 6px;
  overflow: hidden;
  border: 1px solid var(--color-rule);
  background: var(--color-paper-dark);
  padding: 0;
  cursor: pointer;
  isolation: isolate;
}
.plate--featured { margin-bottom: 1.25rem; }

.plate:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 3px;
}

/* Blurred backdrop layer */
.plate-bg {
  position: absolute;
  inset: 0;
  z-index: 0;
  overflow: hidden;
}
.plate-backdrop {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: blur(28px) saturate(1.15);
  transform: scale(1.15);
  opacity: 0.7;
}
.plate-tint {
  position: absolute;
  inset: 0;
  background: rgba(246, 243, 238, 0.22);
}

/* Frame: centered, contained, padded */
.plate-frame {
  position: relative;
  z-index: 1;
  width: 100%;
  height: 100%;
  display: grid;
  place-items: center;
  padding: 10%;
}
.plate--compact .plate-frame { padding: 8%; }

/* Media stack in the same grid cell so hover GIF lines up with static */
.plate-media {
  grid-area: 1 / 1;
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  display: block;
  border-radius: 3px;
  box-shadow: 0 4px 14px rgba(20, 20, 20, 0.14);
  transition: opacity 0.35s ease, transform 0.4s ease;
}

.plate-gif {
  opacity: 0;
}
.plate-gif.visible { opacity: 1; }

.plate--compact:hover .plate-media {
  transform: scale(1.02);
}

.plate-fallback {
  grid-area: 1 / 1;
  font-family: var(--font-display);
  font-size: 2rem;
  color: var(--color-accent);
  opacity: 0.5;
}

/* ---------- Compact body ---------- */
.project--compact {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.compact-title {
  font-family: var(--font-display);
  font-size: 1.15rem;
  font-weight: 500;
  letter-spacing: -0.01em;
  margin: 0 0 0.3rem;
  color: var(--color-ink);
}
.compact-desc {
  font-size: 0.85rem;
  color: var(--color-ink-soft);
  margin: 0 0 0.6rem;
  line-height: 1.55;
}
.compact-tags {
  display: flex; flex-wrap: wrap; gap: 0.4rem;
}
.compact-tags span {
  font-size: 0.68rem;
  padding: 0.2rem 0.55rem;
  border: 1px solid var(--color-rule);
  border-radius: 100px;
  color: var(--color-ink-soft);
}

@media (max-width: 768px) {
  .project--featured {
    grid-template-columns: 1fr;
    gap: 1rem;
    padding: 2.5rem 0;
  }
  .project-num { font-size: 1.8rem; }
  .project-right { padding-top: 0; }
}
</style>