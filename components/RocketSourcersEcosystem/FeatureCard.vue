<script setup lang="ts">
defineProps<{
  id: string
  title: string
  description?: string
  ctaLabel: string
}>()

const emit = defineEmits<{
  'cta-click': [id: string]
}>()
</script>

<template>
  <article class="feature-card">
    <h3 class="feature-card__title">{{ title }}</h3>
    <p v-if="description" class="feature-card__description">{{ description }}</p>
    <button
      type="button"
      class="feature-card__cta"
      @click="emit('cta-click', id)"
    >
      {{ ctaLabel }}
    </button>
  </article>
</template>

<style scoped>
.feature-card {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 8px;
  background: #fff;
  border: 2px solid #e4e4e4;
  border-radius: 24px;
  padding: 16px 32px;
  box-shadow: var(--card-shadow, 2px 2px 7px rgba(0, 0, 0, 0.1));
}

.feature-card__title {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  line-height: 1.2;
  color: #000;
}

.feature-card__description {
  margin: 0;
  font-size: 16px;
  line-height: 1.3;
  color: #000;
}

.feature-card__cta {
  appearance: none;
  border: none;
  cursor: pointer;
  padding: 10px 24px;
  border-radius: 90px;
  background: linear-gradient(90deg, #ee7203, #d93b09);
  color: #fff;
  font-size: 13px;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-transform: uppercase;
  font-family: inherit;
}

.feature-card__cta:hover {
  filter: brightness(1.05);
}

.feature-card__cta:focus-visible {
  outline: 3px solid #6e140b;
  outline-offset: 2px;
}

/* Compact (mobile grid) variant: smaller type, tighter padding, centered title. */
:global(.ecosystem__compact .feature-card) {
  align-items: center;
  padding: 16px;
  border-radius: 8px;
  gap: 8px;
  width: 100%;
  min-width: 0;
}

:global(.ecosystem__compact .feature-card__title) {
  font-size: 12px;
  text-align: center;
  width: 100%;
}

:global(.ecosystem__compact .feature-card__cta) {
  font-size: 10px;
  padding: 8px 16px;
  max-width: 100%;
  white-space: normal;
  text-align: center;
}
</style>
