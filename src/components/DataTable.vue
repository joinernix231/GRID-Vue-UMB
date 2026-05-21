<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  projects: {
    type: Array,
    default: () => [],
  },
})

const sortKey = ref('name')
const sortDir = ref('asc')
const filterStatus = ref('all')

const statusOptions = ['all', 'En progreso', 'Completado', 'Pendiente']

const statusConfig = {
  'Completado': { class: 'badge--success', icon: '✓' },
  'En progreso': { class: 'badge--warning', icon: '◔' },
  'Pendiente': { class: 'badge--neutral', icon: '○' },
}

function sortBy(key) {
  if (sortKey.value === key) {
    sortDir.value = sortDir.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortKey.value = key
    sortDir.value = 'asc'
  }
}

function sortIcon(key) {
  if (sortKey.value !== key) return '↕'
  return sortDir.value === 'asc' ? '↑' : '↓'
}

const filteredProjects = computed(() => {
  let list = props.projects
  if (filterStatus.value !== 'all') {
    list = list.filter((p) => p.status === filterStatus.value)
  }
  return [...list].sort((a, b) => {
    let valA = a[sortKey.value]
    let valB = b[sortKey.value]
    if (typeof valA === 'string') valA = valA.toLowerCase()
    if (typeof valB === 'string') valB = valB.toLowerCase()
    if (valA < valB) return sortDir.value === 'asc' ? -1 : 1
    if (valA > valB) return sortDir.value === 'asc' ? 1 : -1
    return 0
  })
})
</script>

<template>
  <section class="data-table-wrapper" aria-label="Tabla de proyectos">
    <div class="data-table__toolbar">
      <h2 class="data-table__heading">Proyectos</h2>
      <div class="data-table__filters" role="group" aria-label="Filtrar por estado">
        <button
          v-for="opt in statusOptions"
          :key="opt"
          class="data-table__filter-btn"
          :class="{ 'data-table__filter-btn--active': filterStatus === opt }"
          @click="filterStatus = opt"
          :aria-pressed="filterStatus === opt"
        >
          {{ opt === 'all' ? 'Todos' : opt }}
        </button>
      </div>
    </div>

    <div class="data-table__scroll">
      <table class="data-table" role="table" aria-label="Lista de proyectos">
        <thead>
          <tr role="row">
            <th
              scope="col"
              class="data-table__th data-table__th--sortable"
              @click="sortBy('name')"
              :aria-sort="sortKey === 'name' ? (sortDir === 'asc' ? 'ascending' : 'descending') : 'none'"
            >
              Nombre <span aria-hidden="true">{{ sortIcon('name') }}</span>
            </th>
            <th scope="col" class="data-table__th">Estado</th>
            <th
              scope="col"
              class="data-table__th data-table__th--sortable"
              @click="sortBy('progress')"
              :aria-sort="sortKey === 'progress' ? (sortDir === 'asc' ? 'ascending' : 'descending') : 'none'"
            >
              Progreso <span aria-hidden="true">{{ sortIcon('progress') }}</span>
            </th>
            <th
              scope="col"
              class="data-table__th data-table__th--sortable"
              @click="sortBy('date')"
              :aria-sort="sortKey === 'date' ? (sortDir === 'asc' ? 'ascending' : 'descending') : 'none'"
            >
              Fecha <span aria-hidden="true">{{ sortIcon('date') }}</span>
            </th>
            <th scope="col" class="data-table__th">Responsable</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="project in filteredProjects"
            :key="project.name"
            class="data-table__row"
            role="row"
          >
            <td class="data-table__td data-table__td--name">
              <span class="data-table__project-dot" aria-hidden="true"></span>
              {{ project.name }}
            </td>
            <td class="data-table__td">
              <span
                class="badge"
                :class="statusConfig[project.status]?.class"
                :aria-label="`Estado: ${project.status}`"
              >
                <span aria-hidden="true">{{ statusConfig[project.status]?.icon }}</span>
                {{ project.status }}
              </span>
            </td>
            <td class="data-table__td">
              <div class="data-table__progress" role="group" :aria-label="`Progreso: ${project.progress}%`">
                <div class="data-table__progress-bar" :style="{ width: project.progress + '%' }"></div>
              </div>
              <span class="data-table__progress-label">{{ project.progress }}%</span>
            </td>
            <td class="data-table__td data-table__td--muted">{{ project.date }}</td>
            <td class="data-table__td">
              <span class="data-table__avatar" aria-hidden="true">
                {{ project.assignee.charAt(0) }}
              </span>
              {{ project.assignee }}
            </td>
          </tr>
          <tr v-if="filteredProjects.length === 0">
            <td colspan="5" class="data-table__empty">No hay proyectos que coincidan.</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<style scoped>
.data-table-wrapper {
  background: var(--color-surface);
  border-radius: 12px;
  border: 1px solid var(--color-border);
  overflow: hidden;
}

.data-table__toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.75rem;
  padding: 1.25rem 1.5rem;
  border-bottom: 1px solid var(--color-border);
}

.data-table__heading {
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-text);
  margin: 0;
}

.data-table__filters {
  display: flex;
  gap: 0.375rem;
  flex-wrap: wrap;
}

.data-table__filter-btn {
  padding: 0.3rem 0.75rem;
  font-size: 0.78rem;
  font-weight: 500;
  border-radius: 20px;
  border: 1px solid var(--color-border);
  background: none;
  color: var(--color-text-muted);
  cursor: pointer;
  transition:
    background var(--transition-speed),
    color var(--transition-speed),
    border-color var(--transition-speed);
}

.data-table__filter-btn:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.data-table__filter-btn--active {
  background: var(--color-primary);
  color: #fff;
  border-color: var(--color-primary);
}

.data-table__filter-btn:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

.data-table__scroll {
  overflow-x: auto;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.875rem;
}

.data-table__th {
  padding: 0.75rem 1.5rem;
  text-align: left;
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--color-text-muted);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  background: var(--color-bg);
  white-space: nowrap;
}

.data-table__th--sortable {
  cursor: pointer;
  user-select: none;
  transition: color var(--transition-speed);
}

.data-table__th--sortable:hover {
  color: var(--color-primary);
}

.data-table__th--sortable:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: -2px;
}

.data-table__row {
  border-top: 1px solid var(--color-border);
  transition: background var(--transition-speed);
}

.data-table__row:hover {
  background: var(--color-bg);
}

.data-table__td {
  padding: 1rem 1.5rem;
  color: var(--color-text);
  vertical-align: middle;
  white-space: nowrap;
}

.data-table__td--name {
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.data-table__project-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--color-primary);
  flex-shrink: 0;
}

.data-table__td--muted {
  color: var(--color-text-muted);
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  padding: 0.25rem 0.625rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
}

.badge--success {
  background: rgba(45, 106, 79, 0.12);
  color: var(--color-primary);
}

.badge--warning {
  background: rgba(245, 158, 11, 0.12);
  color: #b45309;
}

.badge--neutral {
  background: rgba(107, 114, 128, 0.12);
  color: #6b7280;
}

.data-table__progress {
  width: 80px;
  height: 6px;
  background: var(--color-border);
  border-radius: 3px;
  overflow: hidden;
  display: inline-block;
  margin-right: 0.5rem;
  vertical-align: middle;
}

.data-table__progress-bar {
  height: 100%;
  background: var(--color-primary);
  border-radius: 3px;
  transition: width 0.4s ease;
}

.data-table__progress-label {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  vertical-align: middle;
}

.data-table__avatar {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  border-radius: 50%;
  background: var(--color-primary);
  color: #fff;
  font-size: 0.7rem;
  font-weight: 700;
  margin-right: 0.4rem;
  vertical-align: middle;
}

.data-table__empty {
  text-align: center;
  padding: 2rem;
  color: var(--color-text-muted);
  font-style: italic;
}
</style>
