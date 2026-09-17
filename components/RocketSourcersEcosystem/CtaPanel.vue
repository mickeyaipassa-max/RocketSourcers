<script setup lang="ts">
export interface CtaPanelItem {
  id: string
  icon: 'search' | 'phone' | 'user' | 'pie-chart'
  label: string
}

defineProps<{
  items: CtaPanelItem[]
  taglineLead: string
  taglineRest: string
}>()

const ICON_PATHS: Record<CtaPanelItem['icon'], string[]> = {
  search: [
    'M11 19C15.4183 19 19 15.4183 19 11C19 6.58172 15.4183 3 11 3C6.58172 3 3 6.58172 3 11C3 15.4183 6.58172 19 11 19Z',
    'M21 21L16.65 16.65',
  ],
  phone: [
    'M22 16.92V19.92C22.0011 20.1985 21.9441 20.4742 21.8325 20.7293C21.7209 20.9845 21.5573 21.2136 21.3521 21.4019C21.1469 21.5901 20.9046 21.7335 20.6407 21.8227C20.3769 21.9119 20.0974 21.9451 19.82 21.92C16.7428 21.5856 13.787 20.5341 11.19 18.85C8.77383 17.3147 6.72534 15.2662 5.19 12.85C3.49998 10.2412 2.44824 7.27099 2.12 4.18C2.09501 3.90347 2.12787 3.62476 2.2165 3.36162C2.30513 3.09849 2.44757 2.85669 2.63477 2.65162C2.82196 2.44655 3.0498 2.28271 3.30379 2.17052C3.55778 2.05833 3.83234 2.00026 4.11 2H7.11C7.59531 1.99522 8.06579 2.16708 8.43376 2.48353C8.80173 2.79999 9.04208 3.23945 9.11 3.72C9.23662 4.68007 9.47145 5.62273 9.81 6.53C9.94454 6.88792 9.97366 7.27691 9.89391 7.65088C9.81415 8.02485 9.62886 8.36811 9.36 8.64L8.09 9.91C9.51356 12.4135 11.5865 14.4864 14.09 15.91L15.36 14.64C15.6319 14.3711 15.9752 14.1858 16.3491 14.1061C16.7231 14.0263 17.1121 14.0555 17.47 14.19C18.3773 14.5286 19.3199 14.7634 20.28 14.89C20.7658 14.9585 21.2094 15.2032 21.5265 15.5775C21.8437 15.9518 22.0122 16.4296 22 16.92Z',
  ],
  user: [
    'M20 21V19C20 17.9391 19.5786 16.9217 18.8284 16.1716C18.0783 15.4214 17.0609 15 16 15H8C6.93913 15 5.92172 15.4214 5.17157 16.1716C4.42143 16.9217 4 17.9391 4 19V21',
    'M12 11C14.2091 11 16 9.20914 16 7C16 4.79086 14.2091 3 12 3C9.79086 3 8 4.79086 8 7C8 9.20914 9.79086 11 12 11Z',
  ],
  'pie-chart': [
    'M21.21 15.89C20.5738 17.3945 19.5788 18.7202 18.3119 19.7513C17.045 20.7824 15.5448 21.4874 13.9424 21.8048C12.3401 22.1221 10.6844 22.0421 9.12015 21.5718C7.55587 21.1014 6.13062 20.2551 4.96902 19.1067C3.80741 17.9582 2.94481 16.5428 2.45663 14.9839C1.96846 13.4251 1.86956 11.7705 2.1686 10.1646C2.46763 8.55878 3.1555 7.05063 4.17205 5.77203C5.1886 4.49343 6.50288 3.48332 8 2.83',
    'M22 12C22 10.6868 21.7413 9.38642 21.2388 8.17317C20.7362 6.95991 19.9997 5.85752 19.0711 4.92893C18.1425 4.00035 17.0401 3.26375 15.8268 2.7612C14.6136 2.25866 13.3132 2 12 2V12H22Z',
  ],
}
</script>

<template>
  <div class="cta-panel">
    <div class="cta-panel__inner">
      <ul class="cta-panel__items">
        <li v-for="item in items" :key="item.id" class="cta-panel__item">
          <svg class="cta-panel__icon" viewBox="0 0 24 24" fill="none" aria-hidden="true">
            <path
              v-for="(d, i) in ICON_PATHS[item.icon]"
              :key="i"
              :d="d"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
          <span class="cta-panel__label">{{ item.label }}</span>
        </li>
      </ul>
      <p class="cta-panel__tagline">
        <strong>{{ taglineLead }}</strong> {{ taglineRest }}
      </p>
    </div>
  </div>
</template>

<style scoped>
/*
 * An element's own `container-type` establishes a containment context for
 * its DESCENDANTS, not itself — a `@container` rule targeting this same
 * element would resolve against the next ancestor container instead. So
 * the container lives on `.cta-panel`, and everything that needs to react
 * to its width lives in the `.cta-panel__inner` child below.
 */
.cta-panel {
  container-type: inline-size;
  width: 100%;
  height: 100%;
}

.cta-panel__inner {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16px;
  color: #fff;
}

.cta-panel__items {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  width: 100%;
}

.cta-panel__item {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  flex: 1 0 0;
  min-width: 0;
}

.cta-panel__item + .cta-panel__item::before {
  content: '+';
  position: absolute;
  top: -18px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 16px;
  font-weight: 600;
  line-height: 1;
  color: #fff;
}

.cta-panel__icon {
  width: 24px;
  height: 24px;
  flex-shrink: 0;
}

.cta-panel__label {
  font-size: 12px;
  font-weight: 500;
  line-height: 1.2;
  text-align: center;
  white-space: nowrap;
}

.cta-panel__tagline {
  margin: 0;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.6);
  width: 100%;
  text-align: center;
  font-size: 12px;
  line-height: 1.2;
}

/* Wide container (bottom-bar placement): lay items out in a row with dividers. */
@container (min-width: 480px) {
  .cta-panel__inner {
    flex-direction: row;
    justify-content: center;
  }

  .cta-panel__items {
    flex-direction: row;
    flex: 1 1 auto;
  }

  .cta-panel__item + .cta-panel__item::before {
    top: 50%;
    left: -12px;
    transform: translate(-50%, -50%);
  }

  .cta-panel__tagline {
    padding-top: 0;
    padding-left: 24px;
    border-top: none;
    border-left: 1px solid rgba(255, 255, 255, 0.6);
    text-align: left;
    flex: 0 0 auto;
    max-width: 220px;
  }
}
</style>
