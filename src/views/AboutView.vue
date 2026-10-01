<script setup lang="ts">
import { useTheme } from 'vuetify'
import { computed, onMounted, ref } from 'vue'

const theme = useTheme()
const isDark = ref(theme.global.current.value.dark)

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
    <v-main class="d-flex align-center justify-center pa-4">
      <v-container fluid class="d-flex justify-center">
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6" lg="4">
            <v-card max-width="480" class="mx-auto pa-6 rounded-xl position-relative" elevation="8">
              <v-btn
                class="position-absolute"
                :icon="isDark ? 'mdi-white-balance-sunny' : 'mdi-moon-waning-crescent'"
                variant="text"
                size="small"
                :aria-label="themeLabel"
                @click="toggleTheme"
                style="top: 16px; right: 16px;"
              />

              <div class="d-flex justify-center">
                <v-avatar size="120" color="blue-grey-lighten-2" class="mb-5 text-h5 font-weight-bold">
                  KN
                </v-avatar>
              </div>

              <div class="text-center">
                <p class="text-overline text-primary mb-2">About</p>
                <h1 class="text-h3 font-weight-bold mb-3">Kiyomi Negi-Tran</h1>
                <p class="text-body-1 text-medium-emphasis">
                  I’m a multidisciplinary designer and developer who loves turning complex ideas into
                  clear, human-centered experiences. My work blends visual storytelling, thoughtful UX,
                  and practical product thinking to build digital experiences that feel both useful and
                  memorable.
                </p>
              </div>

              <div class="d-flex justify-center mt-6">
                <v-btn to="/" variant="tonal" rounded="lg">Back home</v-btn>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
