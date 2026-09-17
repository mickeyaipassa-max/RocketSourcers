<script setup lang="ts">
import EcosystemFeatureCard from './RocketSourcersEcosystem/FeatureCard.vue'
import EcosystemCtaPanel, { type CtaPanelItem } from './RocketSourcersEcosystem/CtaPanel.vue'

// Bound via `:src` (not a static `src="..."`) so Vue's asset-url compiler
// transform leaves this public-folder path alone instead of trying to
// resolve it as a module.
const teamImageSrc = '/images/rocketsourcers-ecosystem/team.jpg'

/**
 * The Figma source renders each named breakpoint (1200-1439, 1440, ...) as
 * its own fixed-pixel snapshot — nothing moves while the viewport is
 * resized within one breakpoint's range; the layout only steps to a new
 * snapshot the moment a breakpoint boundary is crossed. Every position
 * below is stored as a [valueAt1200, valueAt1440] pair; `stepVars()` exposes
 * both as CSS custom properties, and the stylesheet reads `--l0`/`--t0` by
 * default and switches to `--l1`/`--t1` inside the `min-width: 1440px`
 * container query — a snap, not a scroll-linked interpolation.
 */
type Interp = [number, number]

function stepVars(left: Interp, top: Interp): Record<string, string> {
  return {
    '--l0': `${left[0]}px`,
    '--l1': `${left[1]}px`,
    '--t0': `${top[0]}px`,
    '--t1': `${top[1]}px`,
  }
}

function widthVars(width: Interp): Record<string, string> {
  return {
    '--w0': `${width[0]}px`,
    '--w1': `${width[1]}px`,
  }
}

interface Feature {
  id: string
  title: string
  description: string
  ctaLabel: string
  left: Interp
  top: Interp
  width: Interp
}

const features: Feature[] = [
  {
    id: 'direct-sourcing',
    title: 'Direct sourcing',
    description: 'We vinden en benaderen professionals die niet vanzelf bij vacatures uitkomen.',
    ctaLabel: 'Meer bereik',
    left: [453, 429],
    top: [146, 132],
    width: [360, 360],
  },
  {
    id: 'sourcingtechnologie',
    title: 'Sourcingtechnologie',
    description: 'Technologie maakt bereik, context en opvolging schaalbaar.',
    ctaLabel: 'Betere opvolging',
    left: [815, 929],
    top: [394, 394],
    width: [333, 333],
  },
  {
    id: 'talentpooling',
    title: 'Talentpooling & nurturing',
    description: 'Relevant talent blijft dichtbij, ook na de search.',
    ctaLabel: 'Slimmer werken',
    left: [478, 445],
    top: [706, 706],
    // The 1200-1439 source has no fixed width on this one card — Figma
    // reports its rendered frame at 287px (hugging a 223px text column +
    // 32px padding each side). Only the 1440 snapshot fixes it at 360px.
    width: [287, 360],
  },
  {
    id: 'data-talent-intelligence',
    title: 'Data & Talent Intelligence',
    description: 'Iedere search levert data en inzichten voor de volgende search.',
    ctaLabel: 'Meer inzicht',
    left: [80, 80],
    top: [388, 388],
    width: [333, 333],
  },
]

interface Bubble {
  id: string
  text: string
  left: Interp
  top: Interp
}

const bubbles: Bubble[] = [
  { id: 'verbinden', text: 'Verbinden met de juiste mensen', left: [841, 955], top: [165, 165] },
  { id: 'vandaag', text: 'Vandaag contact, morgen impact', left: [860, 974], top: [782, 782] },
  // Per Figma's own metadata this one doesn't shift between 1200 and 1440 at all.
  { id: 'van-data', text: 'Van data naar nieuwe mensen', left: [62, 62], top: [776, 776] },
]

interface Arrow {
  id: string
  /** Center point — the arrow rotates around its own center. */
  cx: Interp
  cy: Interp
  rotation: number
}

/**
 * Small curved accents pointing from an annotation bubble toward its photo
 * cluster. Centers taken directly from Figma's own rotated bounding-box
 * metadata (x + width/2, y + height/2) — van-data and verbinden live inside
 * the "Connector - Left" group and, per that metadata, don't shift between
 * 1200 and 1440 at all; only vandaag (root-level) shifts, by the same
 * uniform +114px as the other root-level elements.
 */
const arrows: Arrow[] = [
  { id: 'van-data', cx: [207.474, 207.474], cy: [713.25, 713.25], rotation: 0 },
  { id: 'verbinden', cx: [912.895, 912.895], cy: [343.479, 343.479], rotation: -152.13 },
  { id: 'vandaag', cx: [876.033, 990.033], cy: [938.156, 938.156], rotation: -120 },
]

const ARROW_PATH =
  'M1.86811 93.7674C1.96522 94.3111 2.48469 94.6731 3.02837 94.576C3.57205 94.4788 3.93406 93.9594 3.83694 93.4157L2.85252 93.5915L1.86811 93.7674ZM53.948 5.09154L43.5842 0L44.3567 11.5211L53.948 5.09154ZM2.85252 93.5915L3.83694 93.4157C1.37605 79.6394 0.707039 59.4088 6.18548 41.8901C11.6496 24.4171 23.1728 9.75654 45.1009 6.68439L44.9622 5.69407L44.8234 4.70374C21.9056 7.91454 9.89522 23.3263 4.27664 41.2932C-1.32766 59.2143 -0.628558 79.7909 1.86811 93.7674L2.85252 93.5915Z'

interface Photo {
  id: string
  left: Interp
  top: Interp
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

function desktopPhoto(id: string, left: Interp, top: Interp, width: number, height: number): Photo {
  const [cropLeft, cropTop, cropWidth, cropHeight] = CROPS[id]!
  return { id, left, top, width, height, cropLeft, cropTop, cropWidth, cropHeight }
}

/**
 * Canvas-absolute pixel positions at the 1200 and 1440 reference widths,
 * taken directly from Figma's metadata. Photos 3/13/2/11/16/12 live inside
 * "Connector - Left" and, per that metadata, don't shift between 1200 and
 * 1440 at all; the rest are root-level and shift by the uniform +114px.
 */
const desktopPhotos: Photo[] = [
  desktopPhoto('2', [314, 314], [569, 569], 76, 70),
  desktopPhoto('3', [813, 813], [287, 287], 61, 67),
  desktopPhoto('4', [249, 363], [227, 227], 92, 101),
  desktopPhoto('5', [870, 984], [665, 665], 45, 50),
  desktopPhoto('6', [915, 1029], [690, 690], 81, 89),
  desktopPhoto('8', [995, 1109], [621, 621], 55, 61),
  desktopPhoto('9', [234, 348], [181, 181], 48.892, 54),
  desktopPhoto('10', [185, 299], [221, 221], 40, 44),
  desktopPhoto('11', [214, 214], [548, 548], 98, 91),
  desktopPhoto('12', [763, 763], [634, 634], 50, 55),
  desktopPhoto('13', [888, 888], [280, 280], 89, 98),
  desktopPhoto('14', [372, 486], [624, 624], 44, 49),
  desktopPhoto('15', [742, 856], [325, 325], 49, 54),
  desktopPhoto('16', [229, 229], [689, 689], 74.914, 69),
]

interface MobilePhoto {
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

function mobilePhoto(id: string, left: number, top: number, width: number, height: number): MobilePhoto {
  const [cropLeft, cropTop, cropWidth, cropHeight] = CROPS[id]!
  return { id, left, top, width, height, cropLeft, cropTop, cropWidth, cropHeight }
}

/** Positions within the compact hub-visual zone (400x380 design canvas), as % — this zone is genuinely fluid. */
const mobilePhotos: MobilePhoto[] = [
  mobilePhoto('2', 44.5, 79.474, 8, 7.895),
  mobilePhoto('3', 88.5, 53.947, 5.25, 6.053),
  mobilePhoto('5', 8.25, 67.895, 7.75, 8.947),
  mobilePhoto('6', 75.75, 55.263, 12.75, 14.737),
  mobilePhoto('8', 79.75, 46.316, 10.25, 11.842),
  mobilePhoto('11', 11.5, 57.895, 11.75, 11.316),
  mobilePhoto('13', 45.5, 33.421, 7.25, 8.421),
  mobilePhoto('15', 19.5, 47.632, 6.25, 7.368),
  mobilePhoto('16', 9, 50.526, 7.5, 7.368),
]

const ctaItems: CtaPanelItem[] = [
  { id: 'reach', icon: 'search', label: 'Meer bereik' },
  { id: 'followup', icon: 'phone', label: 'Betere opvolging' },
  { id: 'talent', icon: 'user', label: 'Meer uit bekend talent' },
  { id: 'insight', icon: 'pie-chart', label: 'Meer inzicht' },
]

/** Connector line paths, in the shared 1000x1000 local coordinate space (unaffected by the outer shift). */
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

/** Box (connectors + hub ring) canvas-absolute position; the box's own 1000x1000 content is fixed-size. */
const connectorsBoxLeft: Interp = [-29, 85]
const connectorsBoxTop: Interp = [6, 6]

/** Hub circle canvas-absolute position; size is fixed (324x324) at every width. */
const circleLeft: Interp = [435, 549]
const circleTop: Interp = [341, 341]

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
          aria-hidden="true"
          :style="stepVars(connectorsBoxLeft, connectorsBoxTop)"
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
          <!-- Figma's "Photo Connectors" group sits 4px below "Ecosystem Connectors" (y=10 vs y=6) despite sharing the same x, so its ticks get their own offset. -->
          <g transform="translate(0, 4)">
            <path
              v-for="d in PHOTO_TICKS"
              :key="d"
              :d="d"
              fill="none"
              stroke="#F5996B"
              stroke-width="1.5"
            />
          </g>

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
          :style="[stepVars(p.left, p.top), { width: p.width + 'px', height: p.height + 'px' }]"
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

        <div
          class="ecosystem__hub-copy"
          :style="[stepVars(circleLeft, circleTop), { width: '324px', height: '324px' }]"
        >
          <h2 class="ecosystem__title">RocketSourcers ecosysteem</h2>
          <p class="ecosystem__badge">Talent + relaties + kennis</p>
        </div>

        <div
          v-for="feature in features"
          :key="feature.id"
          class="ecosystem__card-slot"
          :style="[stepVars(feature.left, feature.top), widthVars(feature.width)]"
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
          :style="stepVars(bubble.left, bubble.top)"
        >
          {{ bubble.text }}
        </p>

        <svg
          v-for="arrow in arrows"
          :key="arrow.id"
          class="ecosystem__arrow"
          viewBox="0 0 53.948 94.5917"
          aria-hidden="true"
          :style="[
            stepVars(arrow.cx, arrow.cy),
            { transform: `translate(-50%, -50%) rotate(${arrow.rotation}deg)` },
          ]"
        >
          <path :d="ARROW_PATH" />
        </svg>
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
    padding: 48px 0;
  }

  /*
   * A fixed-pixel canvas, not a proportionally-scaling box: this is the
   * 1200-1439 snapshot's own width. It does not track the container width
   * at all — resizing within 1200-1439 changes only the surrounding
   * margin (via `align-items: center` on `.ecosystem__wide`), never the
   * diagram itself. It steps to the 1440 snapshot's width in one jump at
   * the next container-query breakpoint below.
   */
  .ecosystem__diagram {
    position: relative;
    width: 1200px;
    height: 900px;
  }

  .ecosystem__connectors {
    position: absolute;
    left: var(--l0);
    top: var(--t0);
    width: 1000px;
    height: 1000px;
    overflow: visible;
  }

  .ecosystem__ring {
    transform-box: fill-box;
    transform-origin: center;
    animation: ecosystem-ring-orbit 12s linear infinite;
    filter: drop-shadow(0 0 10px rgba(255, 183, 97, 0.45));
  }

  .ecosystem__wide .ecosystem__hub-copy,
  .ecosystem__card-slot,
  .ecosystem__bubble,
  .ecosystem__arrow {
    position: absolute;
    left: var(--l0);
    top: var(--t0);
  }

  .ecosystem__card-slot {
    width: var(--w0);
  }

  .ecosystem__wide .ecosystem__photo {
    left: var(--l0);
    top: var(--t0);
  }

  .ecosystem__arrow {
    width: 52.948px;
    height: 88.5px;
    fill: #000;
    pointer-events: none;
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

/* ---------- 1440: step to the 1440 snapshot's fixed positions ---------- */
@container ecosystem (min-width: 1440px) {
  .ecosystem__diagram {
    width: 1440px;
  }

  .ecosystem__connectors,
  .ecosystem__wide .ecosystem__hub-copy,
  .ecosystem__card-slot,
  .ecosystem__bubble,
  .ecosystem__arrow,
  .ecosystem__wide .ecosystem__photo {
    left: var(--l1);
    top: var(--t1);
  }

  .ecosystem__card-slot {
    width: var(--w1);
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
