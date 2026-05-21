<script setup>
import StatsCard from '../components/StatsCard.vue'
import DataTable from '../components/DataTable.vue'
import ProgressChart from '../components/ProgressChart.vue'

const stats = [
  {
    title: 'Proyectos en cartera',
    value: 24,
    icon: '📁',
    color: 'green',
    trend: '+4 este mes',
    trendUp: true,
    subtitle: 'Registrados en todo el país',
  },
  {
    title: 'En operación',
    value: 12,
    icon: '🚀',
    color: 'teal',
    trend: '+2 esta semana',
    trendUp: true,
    subtitle: 'Bogotá, Medellín y Cali',
  },
  {
    title: 'Entregas pendientes',
    value: 8,
    icon: '⏳',
    color: 'orange',
    trend: '-3 cerradas',
    trendUp: false,
    subtitle: 'Por cerrar esta quincena',
  },
  {
    title: 'Talento vinculado',
    value: 15,
    icon: '👥',
    color: 'blue',
    trend: '+1 nuevo',
    trendUp: true,
    subtitle: 'Profesionales activos',
  },
]

const projects = [
  {
    name: 'Integración facturación electrónica DIAN',
    status: 'En progreso',
    progress: 74,
    date: '8 may 2026',
    assignee: 'Catalina Ramírez',
  },
  {
    name: 'Onboarding proveedores zona franca',
    status: 'Completado',
    progress: 100,
    date: '10 may 2026',
    assignee: 'Mateo Vásquez',
  },
  {
    name: 'Panel exportadores Eje Cafetero',
    status: 'En progreso',
    progress: 45,
    date: '11 may 2026',
    assignee: 'Daniela Ortega',
  },
  {
    name: 'Optimización tienda en línea Caribe',
    status: 'Pendiente',
    progress: 0,
    date: '16 may 2026',
    assignee: 'Sebastián Muñoz',
  },
  {
    name: 'Pruebas app POS multiplataforma',
    status: 'Pendiente',
    progress: 20,
    date: '18 may 2026',
    assignee: 'Laura Castaño',
  },
]

const weeklyData = [
  { day: 'Lun', tasks: 8 },
  { day: 'Mar', tasks: 15 },
  { day: 'Mié', tasks: 11 },
  { day: 'Jue', tasks: 18 },
  { day: 'Vie', tasks: 14 },
  { day: 'Sáb', tasks: 5 },
  { day: 'Dom', tasks: 3 },
]

const recentActivity = [
  { id: 1, user: 'Mateo Vásquez', action: 'completó', target: 'Onboarding proveedores zona franca', time: 'hace 2 h', icon: '✅' },
  { id: 2, user: 'Catalina Ramírez', action: 'actualizó', target: 'Integración facturación electrónica DIAN', time: 'hace 3 h', icon: '✏️' },
  { id: 3, user: 'Daniela Ortega', action: 'comentó en', target: 'Panel exportadores Eje Cafetero', time: 'hace 5 h', icon: '💬' },
  { id: 4, user: 'Sebastián Muñoz', action: 'creó', target: 'Optimización tienda en línea Caribe', time: 'ayer', icon: '🆕' },
  { id: 5, user: 'Laura Castaño', action: 'inició', target: 'Pruebas app POS multiplataforma', time: 'ayer', icon: '▶️' },
]
</script>

<template>
  <main class="dashboard" role="main" id="main-content">
    <div class="dashboard__hero">
      <div>
        <h1 class="dashboard__title">Dashboard</h1>
        <p class="dashboard__subtitle">Bienvenido de nuevo, Joiner. Aquí tienes el resumen de hoy.</p>
      </div>
      <time class="dashboard__date" datetime="2026-05-14">14 may 2026</time>
    </div>

    <section aria-label="Resumen estadístico">
      <div class="dashboard__stats">
        <StatsCard
          v-for="stat in stats"
          :key="stat.title"
          v-bind="stat"
        />
      </div>
    </section>

    <div class="dashboard__grid">
      <DataTable :projects="projects" />

      <div class="dashboard__side">
        <ProgressChart title="Actividad semanal" :data="weeklyData" />

        <section class="activity" aria-label="Actividad reciente">
          <h2 class="activity__title">Actividad reciente</h2>
          <ol class="activity__list" role="list">
            <li
              v-for="item in recentActivity"
              :key="item.id"
              class="activity__item"
            >
              <span class="activity__icon" aria-hidden="true">{{ item.icon }}</span>
              <div class="activity__content">
                <p class="activity__text">
                  <strong>{{ item.user }}</strong>
                  {{ item.action }}
                  <em>{{ item.target }}</em>
                </p>
                <time class="activity__time">{{ item.time }}</time>
              </div>
            </li>
          </ol>
        </section>
      </div>
    </div>
  </main>
</template>

<style scoped>
.dashboard {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
  box-sizing: border-box;
}

.dashboard__hero {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.dashboard__title {
  font-size: 1.6rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 0 0 0.25rem;
}

.dashboard__subtitle {
  font-size: 0.875rem;
  color: var(--color-text-muted);
  margin: 0;
}

.dashboard__date {
  font-size: 0.825rem;
  color: var(--color-text-muted);
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  padding: 0.35rem 0.75rem;
  border-radius: 20px;
  white-space: nowrap;
  align-self: center;
}

.dashboard__stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.25rem;
}

.dashboard__grid {
  display: grid;
  grid-template-columns: 1fr 340px;
  gap: 1.5rem;
  align-items: start;
}

.dashboard__side {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.activity {
  background: var(--color-surface);
  border-radius: 12px;
  border: 1px solid var(--color-border);
  padding: 1.5rem;
}

.activity__title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-text);
  margin: 0 0 1.25rem;
}

.activity__list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.activity__item {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
}

.activity__icon {
  font-size: 1rem;
  flex-shrink: 0;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--color-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--color-border);
}

.activity__content {
  flex: 1;
}

.activity__text {
  margin: 0;
  font-size: 0.825rem;
  color: var(--color-text);
  line-height: 1.4;
}

.activity__text strong {
  color: var(--color-primary);
}

.activity__text em {
  font-style: normal;
  font-weight: 500;
}

.activity__time {
  font-size: 0.72rem;
  color: var(--color-text-muted);
  margin-top: 0.1rem;
  display: block;
}

@media (max-width: 1100px) {
  .dashboard__grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .dashboard {
    padding: 1.25rem;
    gap: 1.25rem;
  }

  .dashboard__stats {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .dashboard__stats {
    grid-template-columns: 1fr;
  }
}
</style>
