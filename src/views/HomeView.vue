<script setup lang="ts">
import { computed, onMounted } from 'vue'
import { useTheme } from 'vuetify'

const links = [
  { label: 'Portfolio', href: 'https://portfolio.example.com', icon: 'mdi-briefcase-outline' },
  { label: 'Dribbble', href: 'https://dribbble.com', icon: 'mdi-dribbble' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com', icon: 'mdi-linkedin' },
  { label: 'Email', href: 'mailto:hello@example.com', icon: 'mdi-email-outline' },
]

const theme = useTheme()

const isDark = computed({
  get: () => theme.global.current.value.dark,
  set: (value: boolean) => {
    theme.global.name.value = value ? 'dark' : 'light'
    localStorage.setItem('theme', value ? 'dark' : 'light')
  },
})

onMounted(() => {
  const savedTheme = localStorage.getItem('theme')
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  isDark.value = savedTheme ? savedTheme === 'dark' : prefersDark
})

function toggleTheme() {
  isDark.value = !isDark.value
}
</script>

<template>
  <v-app>
    <v-main :class="isDark ? 'd-flex align-center justify-center bg-grey-darken-4' : 'd-flex align-center justify-center bg-grey-lighten-4'">
      <v-container class="py-8" fluid>
        <v-row justify="center">
          <v-col cols="12" sm="10" md="8" lg="6">
            <v-card class="mx-auto pa-6 rounded-xl position-relative" max-width="480" elevation="8">
              <v-btn
                class="position-absolute top-4 right-4"
                color="surface-variant"
                icon
                variant="tonal"
                size="small"
                :aria-label="isDark ? 'Switch to light mode' : 'Switch to dark mode'"
                @click="toggleTheme"
              >
                <v-icon :icon="isDark ? 'mdi-white-balance-sunny' : 'mdi-moon-waning-crescent'" />
              </v-btn>

              <div class="text-center">
                <v-avatar size="104" class="mb-5" color="primary">
                  <span class="text-h5 font-weight-bold">KN</span>
                </v-avatar>

                <h1 class="text-h3 text-md-h2 font-weight-bold mb-3">Kiyomi Negi-Tran</h1>
                <p class="text-body-1 text-medium-emphasis mb-6">
                  Designer, builder, and curious thinker crafting thoughtful digital experiences.
                </p>
              </div>

              <div class="d-flex flex-column ga-3">
                <v-btn
                  v-for="link in links"
                  :key="link.label"
                  :href="link.href"
                  block
                  size="large"
                  rounded="lg"
                  variant="tonal"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <v-icon :icon="link.icon" start />
                  {{ link.label }}
                </v-btn>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
