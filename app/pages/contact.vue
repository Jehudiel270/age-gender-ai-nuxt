<script lang="ts" setup>
const whatsappNumber = '0160329133'
const whatsappLink = `https://wa.me/229${whatsappNumber.replace(/^0/, '')}`

const contactMethods = [
  {
    icon: 'i-simple-icons-whatsapp',
    label: 'WhatsApp',
    value: whatsappNumber,
    href: whatsappLink,
    color: 'success' as const
  },
  {
    icon: 'i-lucide-mail',
    label: 'Email',
    value: 'dossoucarrel@gmail.com',
    href: 'mailto:dossoucarrel@gmail.com',
    color: 'primary' as const
  }
]

const skills = [
  'Deep Learning (TensorFlow / Keras)',
  'Réseaux de neurones convolutifs (CNN)',
  'Python',
  'Développement web (HTML, CSS, JS)',
  'FastAPI',
  'Prompt engineering'
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
        Contact
      </p>
      <h1 class="text-3xl font-bold">
        Discutons de ce projet
      </h1>
      <p class="text-muted mt-2 max-w-xl mx-auto">
        Développeur web passionné par le deep learning — n'hésite pas à me
        contacter pour toute question sur ce projet.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
      <!-- Profil -->
      <div
        v-motion
        :initial="{ opacity: 0, x: -20 }"
        :enter="{ opacity: 1, x: 0, transition: { duration: 500, delay: 100 } }"
      >
        <UCard class="h-full">
          <template #header>
            <div class="flex items-center gap-2 text-lg font-semibold">
              <UIcon
                name="i-lucide-user-circle"
                class="text-primary"
              />
              Profil
            </div>
          </template>

          <p class="font-semibold text-lg mb-1">
            Dossou Sègla Carrel Jéhudiel
          </p>
          <p class="text-sm text-muted mb-4">
            Développeur web & planificateur
          </p>
          <p class="text-sm text-muted mb-6">
            Étudiant passionné par l'apprentissage automatique et le développement
            web, avec un intérêt marqué pour l'application concrète de l'IA à des
            projets utiles.
          </p>

          <p class="text-sm font-semibold mb-2">
            Compétences
          </p>
          <div class="flex flex-wrap gap-2">
            <UBadge
              v-for="skill in skills"
              :key="skill"
              variant="subtle"
              color="neutral"
            >
              {{ skill }}
            </UBadge>
          </div>
        </UCard>
      </div>

      <!-- Moyens de contact -->
      <div
        v-motion
        :initial="{ opacity: 0, x: 20 }"
        :enter="{ opacity: 1, x: 0, transition: { duration: 500, delay: 200 } }"
      >
        <UCard class="ai-glow h-full">
          <template #header>
            <div class="flex items-center gap-2 text-lg font-semibold">
              <UIcon
                name="i-lucide-send"
                class="text-primary"
              />
              Me contacter
            </div>
          </template>

          <div class="flex flex-col gap-4">
            <UButton
              v-for="(method, i) in contactMethods"
              :key="method.label"
              v-motion
              :to="method.href"
              target="_blank"
              :icon="method.icon"
              :color="method.color"
              variant="subtle"
              size="xl"
              block
              :initial="{ opacity: 0, y: 10 }"
              :enter="{ opacity: 1, y: 0, transition: { duration: 300, delay: 300 + i * 100 } }"
            >
              <div class="flex flex-col items-start">
                <span class="text-xs opacity-70">{{ method.label }}</span>
                <span>{{ method.value }}</span>
              </div>
            </UButton>
          </div>

          <USeparator class="my-6" />

          <p class="text-sm text-muted text-center">
            Basé à Cotonou, Bénin 🇧🇯
          </p>
        </UCard>
      </div>
    </div>
  </UContainer>
</template>

<style lang="css" scoped></style>
