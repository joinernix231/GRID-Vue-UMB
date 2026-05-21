<script setup>
import { computed } from 'vue'

const props = defineProps({
  title: {
    type: String,
    default: 'Actividad semanal',
  },
  data: {
    type: Array,
    default: () => [
      { day: 'Lun', tasks: 8 },
      { day: 'Mar', tasks: 15 },
      { day: 'Mié', tasks: 11 },
      { day: 'Jue', tasks: 18 },
      { day: 'Vie', tasks: 14 },
      { day: 'Sáb', tasks: 5 },
      { day: 'Dom', tasks: 3 },
    ],
  },
})

const maxValue = computed(() => Math.max(...props.data.map((d) => d.tasks)))

function barHeight(value) {
  return `${(value / maxValue.value) * 100}%`
}
</script>

<template>
  <section class="chart" aria-label="Gráfico de actividad semanal">
    <div class="chart__header">
      <h2 class="chart__title">{{ title }}</h2>
      <span class="chart__legend">
        <span class="chart__legend-dot" aria-hidden="true"></span>
        Tareas completadas
      </span>
    </div>

    <div class="chart__body" role="img" :aria-label="`Gráfico de barras: ${data.map(d => `${d.day} ${d.tasks} tareas`).join(', ')}`">
      <div class="chart__bars">
        <div
          v-for="item in data"
          :key="item.day"
          class="chart__bar-group"
        >
          <span class="chart__bar-value" aria-hidden="true">{{ item.tasks }}</span>
          <div class="chart__bar-track" aria-hidden="true">
            <div
              class="chart__bar"
              :style="{ height: barHeight(item.tasks) }"
              :class="{ 'chart__bar--peak': item.tasks === maxValue }"
            ></div>
          </div>
          <span class="chart__bar-label">{{ item.day }}</span>
        </div>
      </div>

      <div class="chart__grid" aria-hidden="true">
        <span v-for="n in 4" :key="n" class="chart__grid-line"></span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.chart {
  background: var(--color-surface);
  border-radius: 12px;
  border: 1px solid var(--color-border);
  padding: 1.5rem;
}

.chart__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1.5rem;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.chart__title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-text);
  margin: 0;
}

.chart__legend {
  display: flex;
  align-items: center;
  gap: 0.375rem;
  font-size: 0.78rem;
  color: var(--color-text-muted);
}

.chart__legend-dot {
  width: 10px;
  height: 10px;
  border-radius: 2px;
  background: var(--color-primary);
}

.chart__body {
  position: relative;
}

.chart__grid {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  pointer-events: none;
}

.chart__grid-line {
  display: block;
  width: 100%;
  height: 1px;
  background: var(--color-border);
}

.chart__bars {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 0.5rem;
  height: 160px;
  padding-bottom: 2rem;
  position: relative;
  z-index: 1;
}

.chart__bar-group {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  gap: 0.375rem;
  height: 100%;
}

.chart__bar-value {
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--color-text-muted);
  line-height: 1;
}

.chart__bar-track {
  flex: 1;
  width: 100%;
  display: flex;
  align-items: flex-end;
}

.chart__bar {
  width: 100%;
  background: var(--color-primary-light);
  border-radius: 4px 4px 0 0;
  transition: height 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  min-height: 4px;
}

.chart__bar--peak {
  background: var(--color-primary);
}

.chart__bar:hover {
  filter: brightness(1.15);
}

.chart__bar-label {
  font-size: 0.72rem;
  color: var(--color-text-muted);
  font-weight: 500;
  text-align: center;
  flex-shrink: 0;
}
</style>
