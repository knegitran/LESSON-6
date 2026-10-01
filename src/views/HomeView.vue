<script setup lang="ts">
import { useTheme } from 'vuetify'
import { computed, onMounted, ref } from 'vue'

const theme = useTheme()
const isDark = ref(theme.global.current.value.dark)

const links = [
  { label: 'Portfolio', href: 'https://portfolio.example.com', icon: 'mdi-web' },
  { label: 'Dribbble', href: 'https://dribbble.com', icon: 'mdi-dribbble' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com', icon: 'mdi-linkedin' },
  { label: 'Email', href: 'mailto:hello@example.com', icon: 'mdi-email-outline' },
]

const themeLabel = computed(() => (isDark.value ? 'Switch to light theme' : 'Switch to dark theme'))

onMounted(() => {
  const savedTheme = localStorage.getItem('vuetify-theme')
  if (savedTheme) {
    isDark.value = savedTheme === 'dark'
  }

  theme.global.name.value = isDark.value ? 'dark' : 'light'
})

function toggleTheme() {
  isDark.value = !isDark.value
  theme.global.name.value = isDark.value ? 'dark' : 'light'
  localStorage.setItem('vuetify-theme', isDark.value ? 'dark' : 'light')
}
</script>

<template>
  <v-app>
    <v-main class="d-flex align-center justify-center px-4 py-8">
      <v-container fluid class="d-flex justify-center">
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6" lg="4">
            <v-card class="pa-6 position-relative rounded-xl" max-width="480" elevation="8">
              <v-btn
                class="theme-toggle position-absolute"
                :icon="isDark ? 'mdi-white-balance-sunny' : 'mdi-moon-waning-crescent'"
                variant="text"
                size="small"
                :aria-label="themeLabel"
                @click="toggleTheme"
              />

              <div class="d-flex justify-center">
                <v-avatar size="104" color="deep-purple-lighten-3" class="mb-5 text-h5 font-weight-bold">
                  KN
                </v-avatar>
              </div>

              <div class="text-center mb-6">
                <h1 class="text-h3 font-weight-bold mb-2">Kiyomi Negi-Tran</h1>
                <p class="text-body-1 text-medium-emphasis mb-0">
                  Designer, builder, and curious thinker crafting thoughtful digital experiences.
                </p>
              </div>

              <div class="d-flex flex-column ga-3">
                <v-btn
                  v-for="link in links"
                  :key="link.label"
                  :href="link.href"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="justify-center text-none"
                  variant="tonal"
                  rounded="lg"
                  size="large"
                >
                  <v-icon :icon="link.icon" start />
                  {{ link.label }}
                </v-btn>

                <v-btn to="/about" class="justify-center text-none" variant="tonal" rounded="lg" size="large">
                  <v-icon icon="mdi-account" start />
                  About
                </v-btn>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
.theme-toggle {
  top: 16px;
  right: 16px;
}
</style>
