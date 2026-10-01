<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import { RouterLink } from 'vue-router'

const links = [
  { label: 'Portfolio', href: 'https://portfolio.example.com', emoji: '🌐' },
  { label: 'Dribbble', href: 'https://dribbble.com', emoji: '🎨' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com', emoji: '💼' },
  { label: 'Email', href: 'mailto:hello@example.com', emoji: '✉️' },
]

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
    <section class="profile-card">
      <button class="theme-toggle" type="button" :aria-label="themeLabel" @click="toggleTheme">
        <span aria-hidden="true">{{ isDark ? '☀' : '☾' }}</span>
      </button>

      <div class="profile-photo" aria-label="Profile photo placeholder">
        <span>KN</span>
      </div>

      <div class="profile-details">
        <h1>Kiyomi Negi-Tran</h1>
        <p>Designer, builder, and curious thinker crafting thoughtful digital experiences.</p>
      </div>

      <nav class="link-list" aria-label="Profile links">
        <a
          v-for="link in links"
          :key="link.label"
          :href="link.href"
          class="social-link"
          target="_blank"
          rel="noopener noreferrer"
        >
          <span class="link-emoji" aria-hidden="true">{{ link.emoji }}</span>
          <span>{{ link.label }}</span>
        </a>

        <RouterLink to="/about" class="social-link about-link">
          <span class="link-emoji" aria-hidden="true">👋</span>
          <span>About</span>
        </RouterLink>
      </nav>
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

.profile-card {
  position: relative;
  width: min(100%, 480px);
  padding: 32px 24px 24px;
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
  width: 104px;
  height: 104px;
  margin: 4px auto 20px;
  display: grid;
  place-items: center;
  border-radius: 50%;
  background: linear-gradient(135deg, #f59e0b 0%, #f97316 50%, #a855f7 100%);
  color: #ffffff;
  font-size: 1.75rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  box-shadow: 0 16px 32px rgba(168, 85, 247, 0.28);
}

.profile-details {
  margin-bottom: 28px;
}

.profile-details h1 {
  margin: 0;
  font-size: clamp(2rem, 4vw, 2.5rem);
  line-height: 1.1;
  letter-spacing: -0.05em;
}

.profile-details p {
  margin: 12px auto 0;
  max-width: 34ch;
  color: var(--muted-color);
  font-size: 1rem;
  line-height: 1.6;
}

.link-list {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  min-height: 54px;
  border: 1px solid var(--card-border);
  border-radius: 14px;
  background: var(--surface-bg);
  color: var(--text-color);
  font-weight: 600;
  letter-spacing: 0.02em;
  text-decoration: none;
  transition:
    transform 0.2s ease,
    border-color 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease;
}

.link-emoji {
  font-size: 1.1rem;
  line-height: 1;
}

.social-link:hover {
  transform: translateY(-2px);
  border-color: var(--accent);
  box-shadow: 0 12px 22px rgba(96, 165, 250, 0.16);
  background: var(--surface-hover);
}

@media (max-width: 480px) {
  .profile-card {
    padding-top: 50px;
  }
}
</style>
