<script lang="ts" setup>
// ============================================================
// RÉGLAGES RAPIDES — modifie ces valeurs pour ajuster le rendu
// ============================================================
const MAX_VISIBLE = 12 // nombre d'icônes visibles simultanément
const SPAWN_INTERVAL_MIN = 900 // délai mini entre deux apparitions, en ms
const SPAWN_INTERVAL_MAX = 2200 // délai maxi entre deux apparitions, en ms
const LIFETIME_MIN = 3500 // durée de vie mini d'une icône, en ms
const LIFETIME_MAX = 7500 // durée de vie maxi d'une icône, en ms
const OPACITY_LIGHT_MIN = 25 // opacité mini (%) en mode clair
const OPACITY_LIGHT_MAX = 55 // opacité maxi (%) en mode clair
const OPACITY_DARK_MIN = 35 // opacité mini (%) en mode sombre
const OPACITY_DARK_MAX = 70 // opacité maxi (%) en mode sombre
const FLOAT_DISTANCE_MIN = 14 // amplitude mini du mouvement, en px
const FLOAT_DISTANCE_MAX = 55 // amplitude maxi du mouvement, en px
const SIZE_MIN_PX = 20 // taille mini d'icône, en px
const SIZE_MAX_PX = 80 // taille maxi d'icône, en px

const ICON_POOL = [
  'i-lucide-brain-circuit',
  'i-lucide-scan-face',
  'i-lucide-database',
  'i-lucide-cpu',
  'i-lucide-share-2',
  'i-lucide-sparkles',
  'i-lucide-hash',
  'i-lucide-network',
  'i-lucide-binary',
  'i-lucide-git-branch',
  'i-lucide-atom',
  'i-lucide-layers',
  'i-lucide-scan-line',
  'i-lucide-bot',
  'i-lucide-workflow',
  'i-lucide-boxes',
  'i-lucide-circuit-board',
  'i-lucide-radar',
  'i-lucide-waypoints',
  'i-lucide-orbit'
]

const MOTION_KINDS = ['float', 'spin', 'pulse', 'drift', 'orbit'] as const
type MotionKind = typeof MOTION_KINDS[number]

interface LiveIcon {
  key: number
  name: string
  top: number
  left: number
  sizePx: number
  kind: MotionKind
  duration: number
  floatDistance: number
  driftX: number
  driftY: number
  rotateAmount: number
  scaleAmount: number
  opacityLight: number
  opacityDark: number
  born: number
  lifetime: number
}

// --- Vrai hasard, pas de seed fixe : chaque icône est unique à sa naissance ---
function rand(min: number, max: number) {
  return min + Math.random() * (max - min)
}

function _randInt(min: number, max: number) {
  return Math.floor(rand(min, max + 1))
}

function randomPick<T>(arr: readonly T[]): T {
  return arr[Math.floor(Math.random() * arr.length)]!
}

let uid = 0

function spawnIcon(): LiveIcon {
  const opacityLight = rand(OPACITY_LIGHT_MIN, OPACITY_LIGHT_MAX)
  const opacityDark = rand(OPACITY_DARK_MIN, OPACITY_DARK_MAX)

  return {
    key: uid++,
    name: randomPick(ICON_POOL),
    top: rand(4, 92),
    left: rand(3, 93),
    sizePx: rand(SIZE_MIN_PX, SIZE_MAX_PX),
    kind: randomPick(MOTION_KINDS),
    duration: rand(1400, 3600),
    floatDistance: rand(FLOAT_DISTANCE_MIN, FLOAT_DISTANCE_MAX),
    driftX: rand(-30, 30),
    driftY: rand(-20, 20),
    rotateAmount: rand(15, 50) * (Math.random() < 0.5 ? -1 : 1),
    scaleAmount: rand(1.2, 1.8),
    opacityLight,
    opacityDark,
    born: Date.now(),
    lifetime: rand(LIFETIME_MIN, LIFETIME_MAX)
  }
}

function motionFor(icon: LiveIcon) {
  const base = {
    transition: {
      repeat: Infinity,
      repeatType: 'mirror' as const,
      duration: icon.duration
    }
  }

  switch (icon.kind) {
    case 'float':
      return { y: -icon.floatDistance, ...base }
    case 'spin':
      return { rotate: icon.rotateAmount, y: -icon.floatDistance * 0.3, ...base }
    case 'pulse':
      return { scale: icon.scaleAmount, ...base }
    case 'drift':
      return { x: icon.driftX, y: icon.driftY, ...base }
    case 'orbit':
      return { x: icon.driftX, y: -icon.floatDistance, rotate: icon.rotateAmount * 0.5, ...base }
    default:
      return base
  }
}

const liveIcons = ref<LiveIcon[]>([])
let spawnTimeoutId: ReturnType<typeof setTimeout> | null = null
let sweepIntervalId: ReturnType<typeof setInterval> | null = null

function scheduleNextSpawn() {
  const delay = rand(SPAWN_INTERVAL_MIN, SPAWN_INTERVAL_MAX)
  spawnTimeoutId = setTimeout(() => {
    if (liveIcons.value.length < MAX_VISIBLE) {
      liveIcons.value.push(spawnIcon())
    }
    scheduleNextSpawn()
  }, delay)
}

function sweepExpired() {
  const now = Date.now()
  liveIcons.value = liveIcons.value.filter(icon => now - icon.born < icon.lifetime)
}

onMounted(() => {
  // Peuple immédiatement une partie du plafond pour éviter un écran vide
  // au premier rendu, le reste arrive progressivement via le spawn loop.
  const initialCount = Math.ceil(MAX_VISIBLE * 0.6)
  for (let i = 0; i < initialCount; i++) {
    liveIcons.value.push(spawnIcon())
  }

  scheduleNextSpawn()
  sweepIntervalId = setInterval(sweepExpired, 500)
})

onUnmounted(() => {
  if (spawnTimeoutId) clearTimeout(spawnTimeoutId)
  if (sweepIntervalId) clearInterval(sweepIntervalId)
})
</script>

<template>
  <ClientOnly>
    <div
      class="pointer-events-none fixed inset-0 overflow-hidden z-0"
      aria-hidden="true"
    >
      <TransitionGroup name="icon-fade">
        <div
          v-for="icon in liveIcons"
          :key="icon.key"
          v-motion
          :initial="{ opacity: 0, x: 0, y: 0, rotate: 0, scale: 1 }"
          :enter="{ opacity: 1, ...motionFor(icon) }"
          class="absolute floating-icon"
          :style="{
            'top': `${icon.top}%`,
            'left': `${icon.left}%`,
            'fontSize': `${icon.sizePx}px`,
            '--icon-opacity-light': `${icon.opacityLight}%`,
            '--icon-opacity-dark': `${icon.opacityDark}%`
          }"
        >
          <UIcon :name="icon.name" />
        </div>
      </TransitionGroup>
    </div>
  </ClientOnly>
</template>

<style scoped>
.floating-icon {
  color: var(--ui-primary);
  opacity: var(--icon-opacity-light);
  transition: opacity 1200ms ease;
}

.dark .floating-icon {
  opacity: var(--icon-opacity-dark);
}

.icon-fade-enter-from,
.icon-fade-leave-to {
  opacity: 0 !important;
}
</style>
