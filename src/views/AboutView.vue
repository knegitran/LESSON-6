<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()
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
              <v-avatar size="120" color="primary" class="text-h4 font-weight-bold">
                KN
              </v-avatar>
            </div>

            <div class="text-center mb-4">
              <p class="text-overline text-primary mb-2">About</p>
              <h1 class="text-h3 font-weight-bold mb-3">Kiyomi Negi-Tran</h1>
              <p class="text-body-1 text-medium-emphasis">
                I’m a multidisciplinary designer and developer who loves turning complex ideas into
                clear, human-centered experiences. My work blends visual storytelling, thoughtful UX,
                and practical product thinking to build digital experiences that feel both useful and
                memorable.
              </p>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>
