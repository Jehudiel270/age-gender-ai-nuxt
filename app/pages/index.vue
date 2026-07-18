<script lang="ts" setup></script>

<template>
  <main class="min-h-auto text-default relative">
    <!-- Hero -->
    <section class="relative flex flex-col items-center text-center gap-6 px-6 pt-20 pb-16 max-w-4xl mx-auto">
      <div
        v-motion
        :initial="{ opacity: 0, scale: 0.9 }"
        :enter="{ opacity: 1, scale: 1, transition: { duration: 500 } }"
      >
        <UBadge
          color="primary"
          variant="subtle"
          size="lg"
          class="mb-4"
        >
          <UIcon
            name="i-lucide-sparkles"
            class="mr-1"
          />
          Projet Deep Learning
        </UBadge>
      </div>

      <h1
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 600, delay: 100 } }"
        class="text-4xl md:text-6xl font-bold leading-tight"
      >
        Devinez l'âge et le genre
        <span class="text-primary">d'un visage</span>
        en un instant
      </h1>

      <p
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 600, delay: 200 } }"
        class="text-lg text-muted max-w-2xl"
      >
        Un réseau de neurones convolutif entraîné sur plus de 23 000 visages,
        capable d'estimer l'âge et le genre à partir d'une simple photo —
        directement dans votre navigateur.
      </p>

      <div
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 600, delay: 300 } }"
        class="flex flex-col sm:flex-row gap-4 mt-4"
      >
        <NuxtLink to="/predict">
          <UButton
            size="xl"
            icon="i-lucide-rocket"
            class="shadow-lg"
          >
            Essayer maintenant
          </UButton>
        </NuxtLink>
        <NuxtLink to="/architecture">
          <UButton
            size="xl"
            variant="outline"
            icon="i-lucide-layers"
          >
            Voir l'architecture
          </UButton>
        </NuxtLink>
      </div>
    </section>

    <section class="flex flex-col mx-auto max-w-6xl gap-6 px-6 py-12 relative">
      <div
        v-motion
        :initial="{ opacity: 0, y: -10 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }"
        class="text-lg font-semibold h-4 text-primary"
      >
        Machine Learning
      </div>
      <h2
        v-motion
        :initial="{ opacity: 0, y: 10 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 100 } }"
      >
        CNN-basé sur la prédiction d'âge et de genre avec des images faciales
      </h2>

      <div class="grid md:grid-cols-3 gap-8">
        <!-- Dataset card -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 400, delay: 200 } }"
        >
          <UCard class="pl-6 h-full">
            <template #header>
              <div class="flex text-primary items-center gap-3 text-2xl">
                <UIcon
                  name="i-lucide-database"
                  class="h-13 w-13 text-primary border p-2.5 bg-primary/30 rounded-lg"
                />
                Dataset
              </div>
            </template>
            <p class="text-4xl font-bold">
              UTKFace
            </p>
            <p class="text-muted">
              Dataset de visages pour l'analyse d'âge et de genre.
              <br> 23.708 images
            </p>
          </UCard>
        </div>

        <!-- Age MAE card -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 400, delay: 300 } }"
        >
          <UCard class="pl-6 my-auto h-full">
            <template #header>
              <div class="flex text-primary items-center gap-3 text-2xl">
                <UIcon
                  name="i-lucide-scan-face"
                  class="h-13 w-13 text-primary border p-2.5 bg-primary/30 rounded-lg"
                />
                Age MAE
              </div>
            </template>
            <p class="text-4xl font-bold">
              8,90 ans
            </p>
            <p class="text-muted">
              Mean Absolute Error pour la prédiction de l'âge.
            </p>
          </UCard>
        </div>

        <!-- Gender accuracy card -->
        <div
          v-motion
          :initial="{ opacity: 0, y: 20 }"
          :enter="{ opacity: 1, y: 0, transition: { duration: 400, delay: 400 } }"
        >
          <UCard class="pl-6 my-auto h-full">
            <template #header>
              <div class="flex text-primary items-center gap-3 text-2xl">
                <UIcon
                  name="i-lucide-user"
                  class="h-13 w-13 text-primary border p-2.5 bg-primary/30 rounded-lg"
                />
                Gender Accuracy
              </div>
            </template>
            <p class="text-4xl font-bold">
              85,2%
            </p>
            <p class="text-muted">
              Précision de la prédiction du genre.
            </p>
          </UCard>
        </div>
      </div>

      <div
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 500, delay: 500 } }"
        class="grid md:grid-cols-2 w-full mx-auto mt-8 gap-4"
      >
        <NuxtLink to="/predict">
          <UButton
            class="flex justify-center p-3 text-xl w-full rounded-lg cursor-pointer hover:bg-secondary/20 dark:hover:text-midnight-50 shadow-lg hover:text-primary"
            icon="i-lucide-rocket"
            color="secondary"
            variant="soft"
            block
            :ui="{
              leadingIcon: 'text-primary'
            }"
          >
            Tester le modèle
          </UButton>
        </NuxtLink>
        <NuxtLink to="/architecture">
          <UButton
            class="flex justify-center p-3 text-2xl w-full rounded-lg cursor-pointer"
            variant="outline"
            block
          >
            Voir l'architecture
          </UButton>
        </NuxtLink>
      </div>
    </section>
  </main>
</template>
