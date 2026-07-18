<!-- eslint-disable @typescript-eslint/no-unused-vars -->
<script lang="ts" setup>
const config = useRuntimeConfig()

// Métriques finales du modèle (test set)
const metrics = [
  { label: 'MAE Âge', value: '11,2 ans', icon: 'i-lucide-scan-face' },
  { label: 'Accuracy Genre', value: '88 %', icon: 'i-lucide-user' },
  { label: 'Images d\'entraînement', value: '23 708', icon: 'i-lucide-database' }
]

// Blocs de l'architecture convolutive (pour la frise visuelle)
const convBlocks = [
  { name: 'Bloc 1', filters: 32 },
  { name: 'Bloc 2', filters: 64 },
  { name: 'Bloc 3', filters: 128 },
  { name: 'Bloc 4', filters: 256 }
]

const stack = [
  { label: 'Frontend', value: 'Nuxt 4 + Nuxt UI (Tailwind CSS)', icon: 'i-lucide-layout-panel-left' },
  { label: 'Backend', value: 'FastAPI (Python)', icon: 'i-lucide-server' },
  { label: 'Modèle', value: 'TensorFlow / Keras (CNN multi-output)', icon: 'i-lucide-brain-circuit' },
  { label: 'Détection visage', value: 'OpenCV Haar Cascade', icon: 'i-lucide-scan-face' }
]
</script>

<template>
  <UContainer class="py-12">
    <div
      v-motion
      :initial="{ opacity: 0, y: -20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="text-center mb-12"
    >
      <p class="text-primary font-semibold text-sm uppercase tracking-widest mb-2">
        Deep Learning
      </p>
      <h1 class="text-3xl font-bold">
        Architecture du modèle
      </h1>
      <p class="text-muted mt-2 max-w-2xl mx-auto">
        Un CNN multi-tâches entraîné sur UTKFace pour prédire l'âge et le genre
        à partir d'un visage.
      </p>
    </div>

    <!-- Métriques clés -->
    <div class="grid md:grid-cols-3 gap-6 mb-12">
      <div
        v-for="(m, i) in metrics"
        :key="m.label"
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 400, delay: i * 100 } }"
      >
        <UCard class="ai-glow text-center">
          <UIcon
            :name="m.icon"
            class="text-3xl text-primary mb-2"
          />
          <p class="text-2xl font-bold">
            {{ m.value }}
          </p>
          <p class="text-sm text-muted">
            {{ m.label }}
          </p>
        </UCard>
      </div>
    </div>

    <!-- Téléchargements -->
    <div
      v-motion
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="mb-12"
    >
      <UCard>
        <template #header>
          <div class="flex items-center gap-2 text-lg font-semibold">
            <UIcon
              name="i-lucide-download"
              class="text-primary"
            />
            Ressources du projet
          </div>
        </template>
        <div class="flex flex-col">
          <div class="flex flex-col sm:flex-row gap-4">
            <UButton
              to="/downloads/rapport_projet.pdf"
              target="_blank"
              icon="i-lucide-file-text"
              size="lg"
              block
            >
              Télécharger le rapport (PDF)
            </UButton>
            <UButton
              to="/downloads/Classification_binaire_regression.ipynb"
              target="_blank"
              icon="i-lucide-code"
              size="lg"
              color="neutral"
              variant="subtle"
              block
            >
              Télécharger le notebook (.ipynb)
            </UButton> <br>
          </div>
          <UButton
            to="/downloads/rapport_ecrit.pdf"
            target="_blank"
            icon="i-lucide-file-text"
            size="lg"
            color="success"
            variant="outline"
            block
            class="w-full mt-4"
          >
            Télécharger le rapport écrit  (PDF)
          </UButton>
        </div>
      </UCard>
    </div>

    <!-- Frise architecture convolutive -->
    <div
      v-motion
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="mb-12"
    >
      <UCard>
        <template #header>
          <div class="flex items-center gap-2 text-lg font-semibold">
            <UIcon
              name="i-lucide-layers"
              class="text-primary"
            />
            Tronc convolutif partagé
          </div>
        </template>

        <div class="flex flex-col md:flex-row items-center gap-3 overflow-x-auto py-4">
          <UIcon
            name="i-lucide-image"
            class="text-2xl text-muted shrink-0"
          />
          <UIcon
            name="i-lucide-arrow-right"
            class="text-muted shrink-0"
          />

          <template
            v-for="(block, i) in convBlocks"
            :key="block.name"
          >
            <div
              v-motion
              :initial="{ opacity: 0, scale: 0.8 }"
              :enter="{ opacity: 1, scale: 1, transition: { duration: 300, delay: i * 150 } }"
              class="flex flex-col items-center gap-1 shrink-0"
            >
              <div class="rounded-lg bg-primary/10 border border-primary/30 px-4 py-3 text-center">
                <p class="text-sm font-semibold text-primary">
                  {{ block.name }}
                </p>
                <p class="text-xs text-muted">
                  {{ block.filters }} filtres
                </p>
              </div>
            </div>
            <UIcon
              name="i-lucide-arrow-right"
              class="text-muted shrink-0"
            />
          </template>

          <div
            v-motion
            :initial="{ opacity: 0, scale: 0.8 }"
            :enter="{ opacity: 1, scale: 1, transition: { duration: 300, delay: 700 } }"
            class="flex flex-col items-center gap-1 shrink-0"
          >
            <div class="rounded-lg bg-primary/20 border border-primary/40 px-4 py-3 text-center">
              <p class="text-sm font-semibold text-primary">
                Dense
              </p>
              <p class="text-xs text-muted">
                256 → 128
              </p>
            </div>
          </div>
          <UIcon
            name="i-lucide-arrow-right"
            class="text-muted shrink-0"
          />

          <div
            v-motion
            :initial="{ opacity: 0, scale: 0.8 }"
            :enter="{ opacity: 1, scale: 1, transition: { duration: 300, delay: 850 } }"
            class="flex flex-col gap-2 shrink-0"
          >
            <UBadge
              color="primary"
              variant="subtle"
              size="lg"
            >
              Âge (régression)
            </UBadge>
            <UBadge
              color="secondary"
              variant="subtle"
              size="lg"
            >
              Genre (sigmoid)
            </UBadge>
          </div>
        </div>

        <p class="text-sm text-muted mt-4">
          Chaque bloc convolutif applique une Batch Normalization et une activation ReLU,
          suivies d'un MaxPooling. Le modèle se termine par un Global Average Pooling
          puis deux têtes de sortie spécialisées.
        </p>
      </UCard>
    </div>

    <!-- Stack technique -->
    <div
      v-motion
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="mb-12"
    >
      <UCard>
        <template #header>
          <div class="flex items-center gap-2 text-lg font-semibold">
            <UIcon
              name="i-lucide-blocks"
              class="text-primary"
            />
            Stack technique
          </div>
        </template>

        <div class="grid sm:grid-cols-2 gap-4">
          <div
            v-for="item in stack"
            :key="item.label"
            class="flex items-center gap-3 p-3 rounded-lg bg-elevated"
          >
            <UIcon
              :name="item.icon"
              class="text-xl text-primary shrink-0"
            />
            <div>
              <p class="text-sm font-semibold">
                {{ item.label }}
              </p>
              <p class="text-sm text-muted">
                {{ item.value }}
              </p>
            </div>
          </div>
        </div>
      </UCard>
    </div>

    <!-- Limites, avec honnêteté -->
    <div
      v-motion
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
    >
      <UAlert
        icon="i-lucide-info"
        color="warning"
        variant="subtle"
        title="Limites connues"
        description="Le modèle, entraîné sur des visages déjà recadrés, perd en précision sur des photos où le visage occupe une petite portion de l'image. Une étape de détection et recadrage automatique (OpenCV) a été ajoutée côté API pour limiter cet effet."
      />
    </div>
  </UContainer>
</template>

<style lang="css" scoped></style>
