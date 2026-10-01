<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import { RouterLink } from 'vue-router'

const isDark = ref(true)

const themeLabel = computed(() => (isDark.value ? 'Switch to light mode' : 'Switch to dark mode'))

const applyTheme = () => {
  document.documentElement.setAttribute('data-theme', isDark.value ? 'dark' : 'light')
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

onMounted(() => {
  const savedTheme = localStorage.getItem('theme')
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  isDark.value = savedTheme ? savedTheme === 'dark' : prefersDark
  applyTheme()
})

watch(isDark, () => {
  applyTheme()
})

function toggleTheme() {
  isDark.value = !isDark.value
}
</script>

<template>
  <main class="page-shell">
    <section class="about-card">
      <button class="theme-toggle" type="button" :aria-label="themeLabel" @click="toggleTheme">
        <span aria-hidden="true">{{ isDark ? '☀' : '☾' }}</span>
      </button>

      <div class="profile-photo" aria-label="Portrait photo placeholder">
        <span>KN</span>
      </div>

      <div class="about-content">
        <p class="eyebrow">About</p>
        <h1>Kiyomi Negi-Tran</h1>
        <p>
          I’m a multidisciplinary designer and developer who loves turning complex ideas into clear,
          human-centered experiences. My work blends visual storytelling, thoughtful UX, and practical
          product thinking to build digital experiences that feel both useful and memorable.
        </p>

        <RouterLink to="/" class="back-link">
          <span class="back-link-icon" aria-hidden="true">
            <v-icon icon="mdi-home" size="18" />
          </span>
          Back to home
        </RouterLink>
      </div>
    </section>
  </main>
</template>

<style scoped>
.page-shell {
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 24px;
}

.about-card {
  position: relative;
  width: min(100%, 480px);
  padding: 32px 24px 28px;
  border: 1px solid var(--card-border);
  border-radius: 28px;
  background: var(--card-bg);
  box-shadow: 0 24px 60px rgba(15, 23, 42, 0.24);
  text-align: center;
}

.theme-toggle {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 40px;
  height: 40px;
  border: 1px solid var(--card-border);
  border-radius: 999px;
  background: var(--surface-bg);
  color: var(--text-color);
  cursor: pointer;
  transition:
    transform 0.2s ease,
    border-color 0.2s ease,
    background 0.2s ease;
}

.theme-toggle:hover {
  transform: translateY(-1px);
  border-color: var(--accent);
}

.profile-photo {
  width: 120px;
  height: 120px;
  margin: 8px auto 18px;
  display: grid;
  place-items: center;
  border-radius: 50%;
  background: linear-gradient(135deg, #38bdf8 0%, #8b5cf6 50%, #f97316 100%);
  color: #ffffff;
  font-size: 2rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  box-shadow: 0 16px 32px rgba(59, 130, 246, 0.28);
}

.about-content {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.eyebrow {
  color: var(--accent);
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.about-content h1 {
  margin: 0;
  font-size: clamp(2rem, 4vw, 2.5rem);
  line-height: 1.1;
  letter-spacing: -0.05em;
}

.about-content p {
  margin: 0;
  color: var(--muted-color);
  font-size: 1rem;
  line-height: 1.7;
}

.back-link {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 46px;
  margin-top: 8px;
  padding: 0 20px;
  border-radius: 14px;
  border: 1px solid var(--card-border);
  background: var(--surface-bg);
  color: var(--text-color);
  text-decoration: none;
  font-weight: 600;
  transition:
    transform 0.2s ease,
    border-color 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease;
}

.back-link:hover {
  transform: translateY(-2px);
  border-color: var(--accent);
  box-shadow: 0 12px 22px rgba(96, 165, 250, 0.16);
  background: var(--surface-hover);
}

.back-link-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
}
</style>
