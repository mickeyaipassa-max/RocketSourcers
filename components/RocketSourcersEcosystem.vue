<script setup lang="ts">
import EcosystemFeatureCard from './RocketSourcersEcosystem/FeatureCard.vue'
import EcosystemCtaPanel, { type CtaPanelItem } from './RocketSourcersEcosystem/CtaPanel.vue'

// Bound via `:src` (not a static `src="..."`) so Vue's asset-url compiler
// transform leaves this public-folder path alone instead of trying to
// resolve it as a module.
const teamImageSrc = '/images/rocketsourcers-ecosystem/team.jpg'

interface Feature {
  id: string
  title: string
  description: string
  ctaLabel: string
  /** Position within the 1440x1100 design canvas, expressed as a % of that canvas. */
  desktop: { left: number; top: number; width: number }
}

/**
 * All positions below are percentages of a fixed 1440x1100 design canvas
 * (derived from the supplied Figma Make "1440" breakpoint, which is pixel-
 * identical to the "1200-1439" breakpoint for every element used here).
 * Percentages scale fluidly with the component's own container width via
 * `aspect-ratio` on `.ecosystem__diagram`, instead of swapping between
 * discrete pixel-perfect snapshots per breakpoint.
 */
const features: Feature[] = [
  {
    id: 'direct-sourcing',
    title: 'Direct sourcing',
    description: 'We vinden en benaderen professionals die niet vanzelf bij vacatures uitkomen.',
    ctaLabel: 'Meer bereik',
    desktop: { left: 29.792, top: 12, width: 25 },
  },
  {
    id: 'sourcingtechnologie',
    title: 'Sourcingtechnologie',
    description: 'Technologie maakt bereik, context en opvolging schaalbaar.',
    ctaLabel: 'Betere opvolging',
    desktop: { left: 64.514, top: 35.818, width: 23.125 },
  },
  {
    id: 'talentpooling',
    title: 'Talentpooling & nurturing',
    description: 'Relevant talent blijft dichtbij, ook na de search.',
    ctaLabel: 'Slimmer werken',
    desktop: { left: 30.903, top: 64.182, width: 25 },
  },
  {
    id: 'data-talent-intelligence',
    title: 'Data & Talent Intelligence',
    description: 'Iedere search levert data en inzichten voor de volgende search.',
    ctaLabel: 'Meer inzicht',
    desktop: { left: 5.556, top: 35.273, width: 23.125 },
  },
]

interface Bubble {
  id: string
  text: string
  left: number
  top: number
}

const bubbles: Bubble[] = [
  { id: 'verbinden', text: 'Verbinden met de juiste mensen', left: 66.319, top: 15 },
  { id: 'vandaag', text: 'Vandaag contact, morgen impact', left: 67.639, top: 71.091 },
  { id: 'van-data', text: 'Van data naar nieuwe mensen', left: 10.208, top: 71.091 },
]

interface Photo {
  id: string
  left: number
  top: number
  width: number
  height: number
  cropLeft: number
  cropTop: number
  cropWidth: number
  cropHeight: number
}

/** Shared avatar crop rectangles (identical across every source breakpoint). */
const CROPS: Record<string, [number, number, number, number]> = {
  '2': [-0.16, -20, 478.62, 346.43],
  '3': [-116.6, -12.16, 542.91, 327.7],
  '4': [-221.08, -12.16, 542.91, 327.7],
  '5': [-327.05, -12.16, 542.91, 327.7],
  '6': [-430.04, -12.16, 542.91, 327.7],
  '8': [-221.08, -114.19, 542.91, 327.7],
  '9': [-327.05, -104.05, 542.91, 327.7],
  '10': [-430.04, -105.41, 542.91, 327.7],
  '11': [-0.16, -122.86, 478.62, 346.43],
  '12': [-116.6, -201.35, 542.91, 327.7],
  '13': [-221.08, -206.08, 542.91, 327.7],
  '14': [-327.05, -206.76, 542.91, 327.7],
  '15': [-430.04, -205.41, 542.91, 327.7],
  '16': [-0.16, -221.43, 478.62, 346.43],
}

function photo(id: string, left: number, top: number, width: number, height: number): Photo {
  const [cropLeft, cropTop, cropWidth, cropHeight] = CROPS[id]!
  return { id, left, top, width, height, cropLeft, cropTop, cropWidth, cropHeight }
}

const desktopPhotos: Photo[] = [
  photo('2', 27.708, 52.273, 5.278, 6.364),
  photo('3', 62.361, 26.636, 4.236, 6.091),
  photo('4', 25.208, 20.636, 6.389, 9.182),
  photo('5', 68.333, 60.455, 3.125, 4.545),
  photo('6', 71.458, 62.727, 5.625, 8.091),
  photo('8', 77.014, 56.455, 3.819, 5.545),
  photo('9', 24.167, 16.455, 3.395, 4.909),
  photo('10', 20.764, 20.091, 2.778, 4),
  photo('11', 20.764, 50.364, 6.806, 8.273),
  photo('12', 58.889, 58.182, 3.472, 5),
  photo('13', 67.569, 26, 6.181, 8.909),
  photo('14', 33.75, 56.727, 3.056, 4.455),
  photo('15', 59.444, 29.545, 3.403, 4.909),
  photo('16', 21.806, 63.182, 5.202, 6.273),
]

/** Positions within the compact hub-visual zone (400x380 design canvas). */
const mobilePhotos: Photo[] = [
  photo('2', 44.5, 79.474, 8, 7.895),
  photo('3', 88.5, 53.947, 5.25, 6.053),
  photo('5', 8.25, 67.895, 7.75, 8.947),
  photo('6', 75.75, 55.263, 12.75, 14.737),
  photo('8', 79.75, 46.316, 10.25, 11.842),
  photo('11', 11.5, 57.895, 11.75, 11.316),
  photo('13', 45.5, 33.421, 7.25, 8.421),
  photo('15', 19.5, 47.632, 6.25, 7.368),
  photo('16', 9, 50.526, 7.5, 7.368),
]

const ctaItems: CtaPanelItem[] = [
  { id: 'reach', icon: 'search', label: 'Meer bereik' },
  { id: 'followup', icon: 'phone', label: 'Betere opvolging' },
  { id: 'talent', icon: 'user', label: 'Meer uit bekend talent' },
  { id: 'insight', icon: 'pie-chart', label: 'Meer inzicht' },
]

/** Connector line paths, in the shared 1000x1000 local coordinate space. */
const CONNECTOR_LINES = [
  'M626 341C636 325 616 300 625.5 284',
  'M785 508C802 495 827 478 844 487.5',
  'M626 652C636 667 631 689 641.5 706',
  'M471.42 507.27C454.72 495.2 430.17 479.42 413.48 488.23',
]

const PHOTO_TICKS = [
  'M874 322.496L889.234 323.694',
  'M302.84 226.65L299.5 220.2',
  'M939.88 692.98L951.42 701.13',
  'M1051.5 614L1051.5 553',
  'M250.78 222.12L267.02 211.6',
  'M815 666.396L890.391 671.684',
  'M410.45 620.43L390 591',
  'M778 359.14L752.46 384.15',
]

const DOT_PATHS = [
  'M625.5 274C616 290 636 315 626 331',
  'M862 487.5C845 478 820 495 803 508',
  'M641.5 739C631 722 636 700 626 685',
  'M390 487.5C407 478 432 495 449 508',
]

const emit = defineEmits<{
  'cta-click': [id: string]
}>()

function onCtaClick(id: string) {
  emit('cta-click', id)
}

// The connector "flow" dots use SMIL (<animateMotion>), which CSS's
// prefers-reduced-motion media query cannot gate directly. Render them
// only once we've confirmed, client-side, that the visitor allows motion.
const allowMotion = ref(false)

onMounted(() => {
  const query = window.matchMedia('(prefers-reduced-motion: reduce)')
  allowMotion.value = !query.matches
  query.addEventListener('change', (e) => {
    allowMotion.value = !e.matches
  })
})
</script>

<template>
  <section class="ecosystem" aria-label="RocketSourcers ecosysteem">
    <!-- ============ Diagram layout: hub + cards + connectors (>=1200 container width) ============ -->
    <div class="ecosystem__wide">
      <div class="ecosystem__diagram">
        <svg
          class="ecosystem__connectors"
          viewBox="0 0 1000 1000"
          preserveAspectRatio="xMidYMid meet"
          aria-hidden="true"
        >
          <defs>
            <linearGradient id="eco-ring-gradient" x1="626" x2="626" y1="304" y2="686" gradientUnits="userSpaceOnUse">
              <stop offset="0" stop-color="#fff" />
              <stop offset="0.45193" stop-color="#FFD797" />
              <stop offset="1" stop-color="#fff" />
            </linearGradient>
            <linearGradient id="eco-dot-gradient" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#EE7203" />
              <stop offset="100%" stop-color="#D93B09" />
            </linearGradient>
          </defs>

          <circle
            class="ecosystem__ring"
            cx="626"
            cy="495"
            r="190.5"
            fill="none"
            stroke="url(#eco-ring-gradient)"
          />

          <path
            v-for="d in CONNECTOR_LINES"
            :key="d"
            :d="d"
            fill="none"
            stroke="#F58C54"
            stroke-width="2"
          />
          <path
            v-for="d in PHOTO_TICKS"
            :key="d"
            :d="d"
            fill="none"
            stroke="#F5996B"
            stroke-width="1.5"
          />

          <template v-if="allowMotion">
            <g v-for="(d, pathIndex) in DOT_PATHS" :key="d">
              <circle v-for="dotIndex in [0, 1]" :key="dotIndex" r="4" fill="url(#eco-dot-gradient)">
                <animateMotion
                  dur="1.8s"
                  repeatCount="indefinite"
                  calcMode="spline"
                  keyTimes="0;1"
                  keySplines="0.42 0 0.58 1"
                  :begin="`${-(dotIndex * 0.9)}s`"
                  :path="d"
                />
                <animate
                  attributeName="opacity"
                  values="0;1;1;0"
                  keyTimes="0;0.12;0.82;1"
                  calcMode="spline"
                  keySplines="0.42 0 0.58 1;0 0 1 1;0.42 0 0.58 1"
                  dur="1.8s"
                  repeatCount="indefinite"
                  :begin="`${-(dotIndex * 0.9)}s`"
                />
              </circle>
            </g>
          </template>
        </svg>

        <div
          v-for="p in desktopPhotos"
          :key="p.id"
          class="ecosystem__photo"
          :style="{ left: p.left + '%', top: p.top + '%', width: p.width + '%', height: p.height + '%' }"
        >
          <img
            :src="teamImageSrc"
            alt=""
            loading="lazy"
            :style="{
              left: p.cropLeft + '%',
              top: p.cropTop + '%',
              width: p.cropWidth + '%',
              height: p.cropHeight + '%',
            }"
          >
        </div>

        <div class="ecosystem__hub-copy" style="left: 38.125%; top: 31%; width: 22.5%; height: 29.455%">
          <h2 class="ecosystem__title">RocketSourcers ecosysteem</h2>
          <p class="ecosystem__badge">Talent + relaties + kennis</p>
        </div>

        <div
          v-for="feature in features"
          :key="feature.id"
          class="ecosystem__card-slot"
          :style="{ left: feature.desktop.left + '%', top: feature.desktop.top + '%', width: feature.desktop.width + '%' }"
        >
          <EcosystemFeatureCard
            :id="feature.id"
            :title="feature.title"
            :description="feature.description"
            :cta-label="feature.ctaLabel"
            @cta-click="onCtaClick"
          />
        </div>

        <p
          v-for="bubble in bubbles"
          :key="bubble.id"
          class="ecosystem__bubble"
          :style="{ left: bubble.left + '%', top: bubble.top + '%' }"
        >
          {{ bubble.text }}
        </p>
      </div>

      <div class="ecosystem__cta-slot ecosystem__cta-slot--wide">
        <EcosystemCtaPanel
          :items="ctaItems"
          tagline-lead="Iedere search vergroot"
          tagline-rest="de waarde van het ecosysteem"
        />
      </div>
    </div>

    <!-- ============ Compact layout: stacked hub + card grid (<1200 container width) ============ -->
    <div class="ecosystem__compact">
      <div class="ecosystem__hub-visual-mobile">
        <div
          v-for="p in mobilePhotos"
          :key="p.id"
          class="ecosystem__photo"
          :style="{ left: p.left + '%', top: p.top + '%', width: p.width + '%', height: p.height + '%' }"
        >
          <img
            :src="teamImageSrc"
            alt=""
            loading="lazy"
            :style="{
              left: p.cropLeft + '%',
              top: p.cropTop + '%',
              width: p.cropWidth + '%',
              height: p.cropHeight + '%',
            }"
          >
        </div>
        <div class="ecosystem__ring-mobile" />
        <div class="ecosystem__hub-copy ecosystem__hub-copy--mobile">
          <h2 class="ecosystem__title">RocketSourcers ecosysteem</h2>
          <p class="ecosystem__badge">Talent + relaties + kennis</p>
        </div>
      </div>

      <div class="ecosystem__grid">
        <EcosystemFeatureCard
          v-for="feature in features"
          :id="feature.id"
          :key="feature.id"
          :title="feature.title"
          :cta-label="feature.ctaLabel"
          @cta-click="onCtaClick"
        />
      </div>

      <div class="ecosystem__cta-slot">
        <EcosystemCtaPanel
          :items="ctaItems"
          tagline-lead="Iedere search vergroot"
          tagline-rest="de waarde van het ecosysteem"
        />
      </div>
    </div>
  </section>
</template>

<style scoped>
.ecosystem {
  --card-shadow: 2px 2px 7px rgba(0, 0, 0, 0.1);
  container-type: inline-size;
  container-name: ecosystem;
  width: 100%;
  background: #fff;
}

:global(.ecosystem),
:global(.ecosystem *),
:global(.ecosystem *::before),
:global(.ecosystem *::after) {
  box-sizing: border-box;
}

.ecosystem__wide,
.ecosystem__compact {
  display: none;
}

/* ---------- Compact (<1200): default, mobile-first ---------- */
.ecosystem__compact {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 32px;
  padding: 24px 16px;
}

.ecosystem__hub-visual-mobile {
  position: relative;
  width: 100%;
  max-width: 400px;
  aspect-ratio: 400 / 380;
}

.ecosystem__ring-mobile {
  position: absolute;
  left: 16%;
  top: 24.211%;
  width: 68.75%;
  height: 72.368%;
  border-radius: 50%;
  border: 1px solid #ffd797;
  filter: drop-shadow(0 0 10px rgba(255, 183, 97, 0.45));
  animation: ecosystem-ring-orbit 12s linear infinite;
}

.ecosystem__grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
  width: 100%;
  max-width: 480px;
}

.ecosystem__compact .ecosystem__cta-slot {
  width: 100%;
  max-width: 360px;
  border-radius: 16px;
  background: linear-gradient(135deg, #ee7203, #d93b09);
  box-shadow: var(--card-shadow);
  padding: 24px;
}

/* ---------- Wide (>=1200): hub diagram ---------- */
@container ecosystem (min-width: 1200px) {
  .ecosystem__compact {
    display: none;
  }

  .ecosystem__wide {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 40px;
    padding: 48px 32px;
  }

  .ecosystem__diagram {
    position: relative;
    width: 100%;
    max-width: 1250px;
    aspect-ratio: 1440 / 1100;
  }

  .ecosystem__connectors {
    position: absolute;
    left: 5.903%;
    top: 0.545%;
    width: 69.444%;
    height: 90.909%;
    overflow: visible;
  }

  .ecosystem__ring {
    transform-box: fill-box;
    transform-origin: center;
    animation: ecosystem-ring-orbit 12s linear infinite;
    filter: drop-shadow(0 0 10px rgba(255, 183, 97, 0.45));
  }

  .ecosystem__hub-copy,
  .ecosystem__card-slot,
  .ecosystem__bubble {
    position: absolute;
  }

  .ecosystem__bubble {
    margin: 0;
    white-space: nowrap;
    background: #fff;
    border-radius: 90px;
    padding: 10px 24px;
    box-shadow: var(--card-shadow);
    font-style: italic;
    font-size: 16px;
    color: #000;
  }

  .ecosystem__cta-slot--wide {
    width: 100%;
    max-width: 826px;
    border-radius: 16px;
    background: linear-gradient(90deg, #ee7203, #d93b09);
    box-shadow: var(--card-shadow);
    padding: 24px;
    height: 90px;
  }
}

/* ---------- Extra-wide (>=1600): CTA panel becomes a side panel ---------- */
@container ecosystem (min-width: 1600px) {
  .ecosystem__wide {
    flex-direction: row;
    align-items: center;
    justify-content: center;
    gap: 48px;
  }

  .ecosystem__diagram {
    max-width: 1000px;
  }

  .ecosystem__cta-slot--wide {
    width: 280px;
    max-width: 280px;
    height: auto;
    align-self: center;
  }
}

.ecosystem__photo {
  position: absolute;
  border-radius: 50%;
  overflow: hidden;
  pointer-events: none;
}

/* Shared by both layout modes; each mode only adds its own position/size. */
.ecosystem__hub-copy {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border-radius: 50%;
  text-align: center;
  background: radial-gradient(circle, #6e140b 19.2%, #962418 45.2%, #a8321d 69.2%, #fc7336 100%);
}

.ecosystem__photo img {
  position: absolute;
  max-width: none;
  animation: ecosystem-photo-fade linear infinite;
  animation-duration: 5s;
}

.ecosystem__title {
  margin: 0;
  font-size: 28px;
  font-weight: 600;
  line-height: 1.2;
  color: #fff;
}

.ecosystem__badge {
  margin: 0;
  padding: 9px 16px;
  border-radius: 90px;
  background: #fc682e;
  color: #fff;
  font-size: 13px;
  font-weight: 600;
  text-transform: uppercase;
  white-space: nowrap;
}

.ecosystem__compact .ecosystem__title,
.ecosystem__hub-copy--mobile .ecosystem__title {
  font-size: 16px;
  text-align: center;
}

.ecosystem__hub-copy--mobile {
  position: absolute;
  left: 29.125%;
  top: 37.632%;
  width: 42.75%;
  height: 45%;
}

.ecosystem__hub-copy--mobile .ecosystem__badge {
  font-size: 10px;
  padding: 6px 12px;
}

@keyframes ecosystem-ring-orbit {
  0% {
    transform: rotate(0deg) scale(1);
  }
  25% {
    transform: rotate(90deg) scale(1.025);
  }
  50% {
    transform: rotate(180deg) scale(1);
  }
  75% {
    transform: rotate(270deg) scale(0.975);
  }
  100% {
    transform: rotate(360deg) scale(1);
  }
}

@keyframes ecosystem-photo-fade {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.35;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ecosystem__ring,
  .ecosystem__ring-mobile,
  .ecosystem__photo img {
    animation: none;
  }
}
</style>
