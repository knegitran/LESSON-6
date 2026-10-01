<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()

const links = [
  { label: 'Portfolio', href: 'https://portfolio.example.com', icon: 'mdi-web' },
  { label: 'Dribbble', href: 'https://dribbble.com', icon: 'mdi-palette' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com', icon: 'mdi-linkedin' },
  { label: 'Email', href: 'mailto:hello@example.com', icon: 'mdi-email-outline' },
]

const isDark = ref(true)

const themeLabel = computed(() => (isDark.value ? 'Switch to light mode' : 'Switch to dark mode'))

const applyTheme = () => {
  const nextTheme = isDark.value ? 'dark' : 'light'
  theme.global.name.value = nextTheme
  localStorage.setItem('theme', nextTheme)
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
  <v-main class="d-flex align-center justify-center bg-grey-darken-4">
    <v-container class="py-10">
      <v-row justify="center">
        <v-col cols="12" sm="8" md="6" lg="4">
          <v-card class="mx-auto pa-6 position-relative" rounded="xl" max-width="480" elevation="8">
            <v-btn
              icon
              variant="tonal"
              class="position-absolute top-4 right-4"
              :aria-label="themeLabel"
              @click="toggleTheme"
            >
              <v-icon>{{ isDark ? 'mdi-white-balance-sunny' : 'mdi-weather-night' }}</v-icon>
            </v-btn>

            <div class="d-flex justify-center mb-5">
              <v-avatar size="104" color="primary" class="text-h5 font-weight-bold">
                KN
              </v-avatar>
            </div>

            <div class="text-center mb-6">
              <h1 class="text-h3 font-weight-bold mb-2">Kiyomi Negi-Tran</h1>
              <p class="text-body-1 text-medium-emphasis">
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
                variant="outlined"
                size="large"
                rounded="lg"
                class="justify-center"
                block
              >
                <v-icon start :icon="link.icon" />
                {{ link.label }}
              </v-btn>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>
