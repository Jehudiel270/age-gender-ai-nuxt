<script lang="ts" setup>
const isMobileMenuOpen = ref(false)
const colorMode = useColorMode()
const route = useRoute()

const links = [
  { to: '/', label: 'Accueil' },
  { to: '/predict', label: 'Prédiction' },
  { to: '/architecture', label: 'Architecture' },
  { to: '/portofolio', label: 'Portofolio' }
]

// Un lien est actif si le chemin correspond exactement (cas de "/"),
// ou si le chemin actuel commence par ce lien (cas de sous-pages futures
// comme /architecture/details).
function isActive(to: string) {
  if (to === '/') return route.path === '/'
  return route.path.startsWith(to)
}

function closeMobileMenu() {
  isMobileMenuOpen.value = false
}

function openMobileMenu() {
  isMobileMenuOpen.value = true
}

// Durée de la vague, en ms — ajuste ici si tu veux plus lent/rapide
const WAVE_DURATION = 2100

function spawnWave(x: number, y: number, endRadius: number) {
  const wave = document.createElement('div')
  wave.style.position = 'fixed'
  wave.style.left = `${x}px`
  wave.style.top = `${y}px`
  wave.style.width = '20px'
  wave.style.height = '20px'
  wave.style.borderRadius = '50%'
  wave.style.border = '3px solid var(--ui-primary)'
  wave.style.boxShadow = '0 0 40px 10px var(--ui-primary)'
  wave.style.transform = 'translate(-50%, -50%)'
  wave.style.pointerEvents = 'none'
  wave.style.zIndex = '999999'
  wave.style.transition = `width ${WAVE_DURATION}ms cubic-bezier(0.22, 0.61, 0.36, 1), height ${WAVE_DURATION}ms cubic-bezier(0.22, 0.61, 0.36, 1), opacity ${WAVE_DURATION}ms ease-out`
  wave.style.opacity = '0.9'
  document.body.appendChild(wave)

  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      wave.style.width = `${endRadius * 2}px`
      wave.style.height = `${endRadius * 2}px`
      wave.style.opacity = '0'
    })
  })

  setTimeout(() => wave.remove(), WAVE_DURATION + 100)
}

async function toggleThemeWithTransition(event: MouseEvent) {
  const isDark = colorMode.value === 'dark'
  const nextTheme = isDark ? 'light' : 'dark'

  const x = event.clientX
  const y = event.clientY
  const endRadius = Math.hypot(
    Math.max(x, window.innerWidth - x),
    Math.max(y, window.innerHeight - y)
  )

  spawnWave(x, y, endRadius)

  if (!document.startViewTransition) {
    colorMode.preference = nextTheme
    return
  }

  const transition = document.startViewTransition(() => {
    colorMode.preference = nextTheme
  })

  try {
    await transition.ready
    document.documentElement.animate(
      {
        clipPath: [
          `circle(0px at ${x}px ${y}px)`,
          `circle(${endRadius}px at ${x}px ${y}px)`
        ]
      },
      {
        duration: WAVE_DURATION,
        easing: 'cubic-bezier(0.22, 0.61, 0.36, 1)',
        pseudoElement: '::view-transition-new(root)'
      }
    )
  } catch {
    // Le thème a déjà changé via colorMode.preference — rien à rattraper.
  }
}
</script>

<template>
  <nav class="h-20 sticky top-0 z-50 px-4 bg-default/80 backdrop-blur-md border-b-2 border-primary flex items-center justify-between md:justify-center">
    <NuxtLink
      to="/"
      class="md:hidden font-semibold text-primary"
    >
      AgeGenderCNN
    </NuxtLink>

    <!-- Menu desktop -->
    <ul class="hidden md:flex items-center gap-x-8">
      <li
        v-for="link in links"
        :key="link.to"
      >
        <NuxtLink :to="link.to">
          <UButton
            variant="ghost"
            class="hover:bg-primary/20"
            size="xl"
            :color="isActive(link.to) ? 'primary' : 'neutral'"
            :class="isActive(link.to) ? 'font-semibold underline underline-offset-8 decoration-2' : ''"
          >
            {{ link.label }}
          </UButton>
        </NuxtLink>
      </li>
      <li>
        <NuxtLink to="/contact">
          <UButton
            color="success"
            :variant="isActive('/contact') ? 'solid' : 'subtle'"
            size="xl"
          >
            Contact
          </UButton>
        </NuxtLink>
      </li>
    </ul>

    <!-- Toggle thème desktop -->
    <ClientOnly>
      <UButton
        :icon="colorMode.value === 'dark' ? 'i-lucide-moon' : 'i-lucide-sun'"
        variant="ghost"
        class="hidden md:flex rounded-full dark:hover:bg-secondary/50 hover:bg-primary/10 absolute right-10 cursor-pointer"
        size="xl"
        aria-label="Changer de thème"
        @click="toggleThemeWithTransition"
      />
    </ClientOnly>

    <!-- Actions mobile : dark mode + hamburger -->
    <div class="flex md:hidden items-center gap-x-2">
      <ClientOnly>
        <UButton
          :icon="colorMode.value === 'dark' ? 'i-lucide-moon' : 'i-lucide-sun'"
          variant="ghost"
          class="rounded-full"
          size="lg"
          aria-label="Changer de thème"
          @click="toggleThemeWithTransition"
        />
      </ClientOnly>
      <UButton
        icon="i-lucide-menu"
        variant="ghost"
        size="lg"
        aria-label="Ouvrir le menu"
        @click="openMobileMenu"
      />
    </div>

    <!-- Tiroir mobile -->
    <USlideover
      v-model:open="isMobileMenuOpen"
      side="right"
    >
      <template #content>
        <div class="flex flex-col p-4 gap-2">
          <div class="flex justify-between items-center mb-4">
            <p class="font-semibold text-primary">
              Menu
            </p>
            <UButton
              icon="i-lucide-x"
              variant="ghost"
              size="sm"
              aria-label="Fermer le menu"
              @click="closeMobileMenu"
            />
          </div>

          <NuxtLink
            v-for="link in links"
            :key="link.to"
            :to="link.to"
            @click="closeMobileMenu"
          >
            <UButton
              variant="ghost"
              size="xl"
              block
              class="justify-start"
              :color="isActive(link.to) ? 'primary' : 'neutral'"
              :ui="isActive(link.to) ? { base: 'font-semibold' } : {}"
            >
              <template #leading>
                <UIcon
                  v-if="isActive(link.to)"
                  name="i-lucide-circle-dot"
                  class="text-primary"
                />
              </template>
              {{ link.label }}
            </UButton>
          </NuxtLink>

          <NuxtLink
            to="/contact"
            @click="closeMobileMenu"
          >
            <UButton
              color="success"
              :variant="isActive('/contact') ? 'solid' : 'subtle'"
              size="xl"
              block
              class="justify-start mt-2"
            >
              Contact
            </UButton>
          </NuxtLink>
        </div>
      </template>
    </USlideover>
  </nav>
</template>

<style lang="css" scoped></style>
