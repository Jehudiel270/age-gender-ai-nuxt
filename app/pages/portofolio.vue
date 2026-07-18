<script lang="ts" setup>
const experiences = [
  {
    title: 'Développeur Web / Stagiaire',
    org: 'Entreprise d\'un ami / Projets personnels',
    period: 'Présent',
    points: [
      'Planification et gestion des activités au sein de l\'entreprise Black Art Entertainment',
      'Développement web via des projets personnels utilisant l\'IA, JavaScript et d\'autres technologies',
      'Utilisation fréquente de l\'IA (5 fois par semaine) avec des prompts sur mesure'
    ]
  }
]

const formations = [
  {
    title: 'BAC D',
    org: 'CPEG Padre Pio',
    period: '2025 – 2026'
  },
  {
    title: 'Formation autodidacte en développement web',
    org: 'Auto-formation',
    period: null
  }
]

const certifications = [
  'Développeur Web — Sololearn',
  'Formation officielle du Miabe Hackathon',
  'Prompt Engineering',
  'Maîtrise du CSS'
]

const skillGroups = [
  {
    title: 'Machine Learning & Deep Learning',
    icon: 'i-lucide-brain-circuit',
    skills: ['TensorFlow / Keras', 'CNN (réseaux convolutifs)', 'Prétraitement de données', 'Prompt Engineering']
  },
  {
    title: 'Développement Web',
    icon: 'i-lucide-code-2',
    skills: ['HTML', 'CSS', 'JavaScript', 'FastAPI', 'Nuxt / Vue']
  },
  {
    title: 'Autres compétences',
    icon: 'i-lucide-sparkles',
    skills: ['Algorithmes (LeetCode / Python)', 'Copywriting', 'Marketing digital', 'Planification de projets']
  }
]

const qualities = [
  'Perspicacité et vitesse d\'analyse',
  'Rapidité d\'adaptation',
  'Très polyvalent',
  'Veille technologique active'
]
</script>

<template>
  <UContainer class="py-12">
    <!-- En-tête profil -->
    <div
      v-motion
      :initial="{ opacity: 0, y: -20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500 } }"
      class="text-center mb-4"
    >
      <p class="text-primary font-semibold text-sm uppercase tracking-widest mb-2">
        Portfolio
      </p>
      <h1 class="text-3xl font-bold">
        Dossou Sègla Carrel Jéhudiel
      </h1>
      <p class="text-muted mt-2">
        Développeur Web & Planificateur — passionné de Deep Learning
      </p>

      <div class="flex justify-center gap-3 mt-6">
        <UButton
          to="/downloads/dossou_carrel_cv.pdf"
          target="_blank"
          icon="i-lucide-download"
          size="lg"
        >
          Télécharger mon CV
        </UButton>
        <UButton
          to="/contact"
          icon="i-lucide-send"
          variant="subtle"
          color="neutral"
          size="lg"
        >
          Me contacter
        </UButton>
      </div>
    </div>

    <!-- Profil texte -->
    <div
      v-motion
      :initial="{ opacity: 0, y: 20 }"
      :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 100 } }"
      class="max-w-2xl mx-auto text-center mb-14"
    >
      <p class="text-muted">
        Étudiant en Licence 1 de Planification à l'ENEAM, je me forme en parallèle
        et en autodidacte au développement web et à l'intelligence artificielle.
        Ce site présente <NuxtLink
          to="/architecture"
          class="text-primary underline"
        >
          un projet de deep learning
        </NuxtLink> que j'ai conçu de bout en bout : de la préparation des données
        jusqu'au déploiement d'une application complète.
      </p>
    </div>

    <div class="grid md:grid-cols-3 gap-8 max-w-5xl mx-auto">
      <!-- Colonne principale -->
      <div class="md:col-span-2 flex flex-col gap-8">
        <!-- Projet phare -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 150 } }"
        >
          <UCard class="ai-glow">
            <template #header>
              <div class="flex items-center gap-2 text-lg font-semibold">
                <UIcon
                  name="i-lucide-star"
                  class="text-primary"
                />
                Projet phare
              </div>
            </template>
            <p class="font-semibold mb-1">
              AgeGenderCNN — Prédiction d'âge et de genre par CNN
            </p>
            <p class="text-sm text-muted mb-4">
              Réseau de neurones convolutif multi-tâches entraîné sur 23 708 images
              (UTKFace), exposé via une API FastAPI et connecté à cette interface Nuxt.
            </p>
            <div class="flex gap-3">
              <UButton
                to="/architecture"
                variant="subtle"
                size="sm"
              >
                Voir l'architecture
              </UButton>
              <UButton
                to="/predict"
                variant="ghost"
                size="sm"
              >
                Tester le modèle
              </UButton>
            </div>
          </UCard>
        </div>

        <!-- Expériences -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 200 } }"
        >
          <UCard>
            <template #header>
              <div class="flex items-center gap-2 text-lg font-semibold">
                <UIcon
                  name="i-lucide-briefcase"
                  class="text-primary"
                />
                Expériences professionnelles
              </div>
            </template>
            <div
              v-for="exp in experiences"
              :key="exp.title"
              class="flex flex-col gap-2"
            >
              <div class="flex justify-between items-baseline flex-wrap gap-1">
                <p class="font-semibold">
                  {{ exp.title }}
                </p>
                <UBadge
                  variant="subtle"
                  color="neutral"
                >
                  {{ exp.period }}
                </UBadge>
              </div>
              <p class="text-sm text-muted">
                {{ exp.org }}
              </p>
              <ul class="text-sm text-muted list-disc list-inside flex flex-col gap-1 mt-1">
                <li
                  v-for="point in exp.points"
                  :key="point"
                >
                  {{ point }}
                </li>
              </ul>
            </div>
          </UCard>
        </div>

        <!-- Formations -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 250 } }"
        >
          <UCard>
            <template #header>
              <div class="flex items-center gap-2 text-lg font-semibold">
                <UIcon
                  name="i-lucide-graduation-cap"
                  class="text-primary"
                />
                Formations
              </div>
            </template>
            <div class="flex flex-col gap-4">
              <div
                v-for="f in formations"
                :key="f.title"
                class="flex justify-between items-baseline flex-wrap gap-1"
              >
                <div>
                  <p class="font-semibold">
                    {{ f.title }}
                  </p>
                  <p class="text-sm text-muted">
                    {{ f.org }}
                  </p>
                </div>
                <UBadge
                  v-if="f.period"
                  variant="subtle"
                  color="neutral"
                >
                  {{ f.period }}
                </UBadge>
              </div>
            </div>
          </UCard>
        </div>
      </div>

      <!-- Colonne latérale -->
      <div class="flex flex-col gap-8">
        <div
          v-for="(group, i) in skillGroups"
          :key="group.title"
          v-motion
          :initial="{ opacity: 0, x: 20 }"
          :enter="{ opacity: 1, x: 0, transition: { duration: 400, delay: 150 + i * 100 } }"
        >
          <UCard>
            <template #header>
              <div class="flex items-center gap-2 font-semibold">
                <UIcon
                  :name="group.icon"
                  class="text-primary"
                />
                {{ group.title }}
              </div>
            </template>
            <div class="flex flex-wrap gap-2">
              <UBadge
                v-for="skill in group.skills"
                :key="skill"
                variant="subtle"
                color="neutral"
              >
                {{ skill }}
              </UBadge>
            </div>
          </UCard>
        </div>

        <div
          v-motion
          :initial="{ opacity: 0, x: 20 }"
          :enter="{ opacity: 1, x: 0, transition: { duration: 400, delay: 450 } }"
        >
          <UCard>
            <template #header>
              <div class="flex items-center gap-2 font-semibold">
                <UIcon
                  name="i-lucide-badge-check"
                  class="text-primary"
                />
                Certifications
              </div>
            </template>
            <ul class="flex flex-col gap-2">
              <li
                v-for="cert in certifications"
                :key="cert"
                class="flex items-center gap-2 text-sm text-muted"
              >
                <UIcon
                  name="i-lucide-check"
                  class="text-primary shrink-0"
                />
                {{ cert }}
              </li>
            </ul>
          </UCard>
        </div>

        <div
          v-motion
          :initial="{ opacity: 0, x: 20 }"
          :enter="{ opacity: 1, x: 0, transition: { duration: 400, delay: 550 } }"
        >
          <UCard>
            <template #header>
              <div class="flex items-center gap-2 font-semibold">
                <UIcon
                  name="i-lucide-sparkles"
                  class="text-primary"
                />
                Qualités
              </div>
            </template>
            <ul class="flex flex-col gap-2">
              <li
                v-for="q in qualities"
                :key="q"
                class="flex items-center gap-2 text-sm text-muted"
              >
                <UIcon
                  name="i-lucide-check"
                  class="text-primary shrink-0"
                />
                {{ q }}
              </li>
            </ul>
          </UCard>
        </div>
      </div>
    </div>
  </UContainer>
</template>

<style lang="css" scoped></style>
