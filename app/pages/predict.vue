<!-- eslint-disable @typescript-eslint/no-unused-vars -->
<script lang="ts" setup>
interface PredictionResponse {
  age: number
  gender: string
  gender_confidence: number
}

const monFichier = ref<File | null>(null)
const isLoading = ref(false)
const errorMessage = ref<string | null>(null)

const age_predict = ref<number | null>(null)
const gender_predict = ref<string | null>(null)
const confiance_predict = ref<number | null>(null)
const inference_time = ref<number | null>(null)

const config = useRuntimeConfig()

// On efface les anciens résultats dès qu'un nouveau fichier est choisi
watch(monFichier, () => {
  age_predict.value = null
  gender_predict.value = null
  confiance_predict.value = null
  inference_time.value = null
  errorMessage.value = null
})

async function lancerPrediction() {
  if (!monFichier.value) return

  isLoading.value = true
  errorMessage.value = null
  const startTime = performance.now()

  try {
    const formData = new FormData()
    formData.append('file', monFichier.value)

    const response = await $fetch<PredictionResponse>(
      `${config.public.apiBase}/predict`,
      {
        method: 'POST',
        body: formData
      }
    )

    age_predict.value = response.age
    gender_predict.value = response.gender
    confiance_predict.value = response.gender_confidence
    inference_time.value = performance.now() - startTime
  } catch (error) {
    errorMessage.value = 'La prédiction a échoué. Vérifie que le serveur est bien lancé.'
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <UContainer class="py-12">
    <div
      v-motion
      :initial="{ opacity: 0, y: -20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="text-center mb-10"
    >
      <p class="text-primary font-semibold text-sm uppercase tracking-widest mb-2">
        Machine Learning
      </p>
      <h1 class="text-3xl font-bold">
        Prédiction d'âge et de genre
      </h1>
      <p class="text-muted mt-2">
        Uploadez une photo de visage pour obtenir une prédiction en temps réel.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
      <!-- Colonne upload -->
      <div
        v-motion
        :initial="{ opacity: 0, x: -20 }"
        :enter="{ opacity: 1, x: 0, transition: { duration: 500, delay: 100 } }"
      >
        <UCard class="ai-glow h-full">
          <template #header>
            <div class="flex items-center gap-2 text-lg font-semibold">
              <UIcon
                name="i-lucide-upload"
                class="text-primary"
              />
              Image source
            </div>
          </template>

          <div class="flex flex-col gap-4">
            <!-- Wrapper relatif pour poser l'overlay de scan par-dessus -->
            <div class="relative">
              <UFileUpload
                v-model="monFichier"
                label="Uploader une image"
                description="JPG, PNG jusqu'à 10 Mo"
                accept="image/*"
                highlight
                :disabled="isLoading"
                class=" transition-2 hover:bg-primary/20"
              />

              <!-- Overlay d'analyse : ligne lumineuse qui balaie l'image -->
              <div
                v-if="isLoading"
                class="absolute inset-0 rounded-lg overflow-hidden pointer-events-none"
              >
                <div class="absolute inset-0 bg-primary/5" />
                <div
                  v-motion
                  :initial="{ top: '0%' }"
                  :enter="{
                    top: ['0%', '100%', '0%'],
                    transition: { duration: 1600, repeat: Infinity, ease: 'easeInOut' }
                  }"
                  class="absolute left-0 right-0 h-0.5 bg-primary shadow-[0_0_12px_2px_var(--ui-primary)]"
                />
              </div>
            </div>

            <UButton
              block
              size="xl"
              icon="i-lucide-scan-face"
              :loading="isLoading"
              :disabled="!monFichier"
              @click="lancerPrediction"
            >
              {{ isLoading ? 'Analyse en cours…' : 'Lancer la prédiction' }}
            </UButton>

            <UAlert
              v-if="errorMessage"
              color="error"
              variant="soft"
              :title="errorMessage"
              icon="i-lucide-alert-triangle"
            />
          </div>
        </UCard>
      </div>

      <!-- Colonne résultats -->
      <div
        v-motion
        :initial="{ opacity: 0, x: 20 }"
        :enter="{ opacity: 1, x: 0, transition: { duration: 500, delay: 200 } }"
      >
        <UCard class="h-full">
          <template #header>
            <div class="flex items-center gap-2 text-lg font-semibold">
              <UIcon
                name="i-lucide-sparkles"
                class="text-primary"
              />
              Résultats
            </div>
          </template>

          <div
            v-if="age_predict !== null"
            v-motion
            :initial="{ opacity: 0, y: 10 }"
            :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }"
            class="flex flex-col gap-5"
          >
            <div>
              <p class="text-sm text-muted mb-1">
                Âge estimé
              </p>
              <p class="text-4xl font-bold text-primary">
                {{ age_predict }} ans
              </p>
            </div>

            <div>
              <p class="text-sm text-muted mb-1">
                Genre
              </p>
              <UBadge
                size="lg"
                variant="subtle"
              >
                {{ gender_predict }}
              </UBadge>
            </div>

            <div>
              <p class="text-sm text-muted mb-1">
                Confiance sur le genre
              </p>
              <UProgress :model-value="(confiance_predict ?? 0) * 100" />
              <p class="text-sm text-muted mt-1">
                {{ ((confiance_predict ?? 0) * 100).toFixed(1) }}%
              </p>
            </div>

            <p class="text-xs text-muted">
              Temps d'inférence : {{ inference_time?.toFixed(0) }} ms
            </p>
          </div>

          <div
            v-else-if="isLoading"
            class="flex flex-col items-center justify-center text-center text-muted h-full py-12 gap-3"
          >
            <UIcon
              name="i-lucide-loader-circle"
              class="text-4xl text-primary animate-spin"
            />
            <p>Analyse du visage en cours…</p>
          </div>

          <div
            v-else
            class="flex flex-col items-center justify-center text-center text-muted h-full py-12 gap-2"
          >
            <UIcon
              name="i-lucide-image"
              class="text-4xl opacity-50"
            />
            <p>Les résultats apparaîtront ici après la prédiction.</p>
          </div>
        </UCard>
      </div>
    </div>
  </UContainer>
</template>

<style lang="css" scoped></style>
