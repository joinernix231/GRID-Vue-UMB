<script setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
  value: {
    type: [String, Number],
    required: true,
  },
  subtitle: {
    type: String,
    default: '',
  },
  icon: {
    type: String,
    default: '📊',
  },
  color: {
    type: String,
    default: 'green',
    validator: (v) => ['green', 'teal', 'blue', 'orange'].includes(v),
  },
  trend: {
    type: String,
    default: '',
  },
  trendUp: {
    type: Boolean,
    default: true,
  },
})
</script>

<template>
  <article class="stats-card" :class="`stats-card--${color}`" tabindex="0">
    <div class="stats-card__header">
      <span class="stats-card__icon" aria-hidden="true">{{ icon }}</span>
      <span v-if="trend" class="stats-card__trend" :class="trendUp ? 'stats-card__trend--up' : 'stats-card__trend--down'">
        <span aria-hidden="true">{{ trendUp ? '↑' : '↓' }}</span>
        {{ trend }}
      </span>
    </div>
    <div class="stats-card__body">
      <p class="stats-card__value" aria-label="`${title}: ${value}`">{{ value }}</p>
      <h3 class="stats-card__title">{{ title }}</h3>
      <p v-if="subtitle" class="stats-card__subtitle">{{ subtitle }}</p>
    </div>
  </article>
</template>

<style scoped>
.stats-card {
  background: var(--color-surface);
  border-radius: 12px;
  padding: 1.5rem;
  border: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  cursor: default;
  transition:
    transform var(--transition-speed),
    box-shadow var(--transition-speed);
  position: relative;
  overflow: hidden;
}

.stats-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  border-radius: 12px 12px 0 0;
}

.stats-card--green::before {
  background: var(--color-primary);
}

.stats-card--teal::before {
  background: #14b8a6;
}

.stats-card--blue::before {
  background: #3b82f6;
}

.stats-card--orange::before {
  background: #f59e0b;
}

.stats-card:hover,
.stats-card:focus-visible {
  transform: translateY(-3px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

.stats-card:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

.stats-card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.stats-card__icon {
  font-size: 1.75rem;
  line-height: 1;
}

.stats-card__trend {
  font-size: 0.75rem;
  font-weight: 600;
  padding: 0.2rem 0.5rem;
  border-radius: 20px;
}

.stats-card__trend--up {
  background: rgba(45, 106, 79, 0.12);
  color: var(--color-primary);
}

.stats-card__trend--down {
  background: rgba(239, 68, 68, 0.1);
  color: #ef4444;
}

.stats-card__body {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.stats-card__value {
  font-size: 2rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 0;
  line-height: 1.1;
}

.stats-card__title {
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--color-text-muted);
  margin: 0;
}

.stats-card__subtitle {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  margin: 0;
}
</style>
