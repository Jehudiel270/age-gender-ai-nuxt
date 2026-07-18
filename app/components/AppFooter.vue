<script lang="ts" setup>
const links = [
  { to: '/', label: 'Accueil' },
  { to: '/predict', label: 'Prédiction' },
  { to: '/architecture', label: 'Architecture' },
  { to: '/portofolio', label: 'Portofolio' },
  { to: '/contact', label: 'Contact' }
]

const year = new Date().getFullYear()
</script>

<template>
  <footer class="border-t border-default mt-16">
    <UContainer class="py-10 flex flex-col md:flex-row items-center justify-between gap-6">
      <div class="text-center md:text-left">
        <p class="font-semibold text-primary">
          AgeGenderCNN
        </p>
        <p class="text-sm text-muted">
          Prédiction d'âge et de genre par deep learning.
        </p>
      </div>

      <ul class="flex flex-wrap justify-center gap-x-6 gap-y-2">
        <li
          v-for="link in links"
          :key="link.to"
        >
          <NuxtLink
            :to="link.to"
            class="text-sm text-muted hover:text-primary transition-colors"
          >
            {{ link.label }}
          </NuxtLink>
        </li>
      </ul>

      <div class="flex items-center gap-3">
        <UButton
          to="https://wa.me/229160329133"
          target="_blank"
          icon="i-simple-icons-whatsapp"
          variant="ghost"
          color="success"
          size="sm"
          aria-label="WhatsApp"
        />
        <UButton
          to="mailto:dossoucarrel@gmail.com"
          icon="i-lucide-mail"
          variant="ghost"
          size="sm"
          aria-label="Email"
        />
      </div>
    </UContainer>

    <USeparator />

    <p class="text-center text-xs text-muted py-4">
      © {{ year }} Dossou Sègla Carrel Jéhudiel — Black Art Entertainment
    </p>
  </footer>
</template>

<style lang="css" scoped></style>
