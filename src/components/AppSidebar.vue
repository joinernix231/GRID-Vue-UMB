<script setup>
import { ref } from 'vue'

const props = defineProps({
  collapsed: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['toggle'])

const navItems = [
  { id: 'dashboard', label: 'Dashboard', icon: '📊', href: '#' },
  { id: 'projects', label: 'Proyectos', icon: '📁', href: '#' },
  { id: 'tasks', label: 'Tareas', icon: '✅', href: '#' },
  { id: 'team', label: 'Equipo', icon: '👥', href: '#' },
  { id: 'analytics', label: 'Analítica', icon: '📈', href: '#' },
  { id: 'settings', label: 'Configuración', icon: '⚙️', href: '#' },
]

const activeItem = ref('dashboard')

function setActive(id) {
  activeItem.value = id
}
</script>

<template>
  <aside
    class="sidebar"
    :class="{ 'sidebar--collapsed': collapsed }"
    role="navigation"
    aria-label="Menú principal"
  >
    <div class="sidebar__brand">
      <span class="sidebar__logo" aria-hidden="true">🌿</span>
      <span v-if="!collapsed" class="sidebar__brand-name">Panel Andino</span>
    </div>

    <nav>
      <ul class="sidebar__nav" role="list">
        <li v-for="item in navItems" :key="item.id" class="sidebar__nav-item">
          <a
            :href="item.href"
            class="sidebar__nav-link"
            :class="{ 'sidebar__nav-link--active': activeItem === item.id }"
            :aria-current="activeItem === item.id ? 'page' : undefined"
            @click.prevent="setActive(item.id)"
            :title="collapsed ? item.label : undefined"
          >
            <span class="sidebar__nav-icon" aria-hidden="true">{{ item.icon }}</span>
            <span v-if="!collapsed" class="sidebar__nav-label">{{ item.label }}</span>
          </a>
        </li>
      </ul>
    </nav>

    <div class="sidebar__footer">
      <a
        href="#"
        class="sidebar__logout"
        @click.prevent
        :title="collapsed ? 'Cerrar sesión' : undefined"
        aria-label="Cerrar sesión"
      >
        <span aria-hidden="true">🚪</span>
        <span v-if="!collapsed">Cerrar sesión</span>
      </a>
    </div>
  </aside>
</template>

<style scoped>
.sidebar {
  grid-area: sidebar;
  background: var(--color-sidebar-bg);
  width: var(--sidebar-width);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  transition: width var(--transition-speed) ease;
  overflow: hidden;
  position: relative;
  z-index: 100;
}

.sidebar--collapsed {
  width: var(--sidebar-collapsed-width);
}

.sidebar__brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1.5rem 1.25rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  min-height: 72px;
  white-space: nowrap;
  overflow: hidden;
}

.sidebar__logo {
  font-size: 1.5rem;
  flex-shrink: 0;
}

.sidebar__brand-name {
  font-size: 1.1rem;
  font-weight: 700;
  color: #fff;
  letter-spacing: 0.5px;
}


.sidebar__nav {
  list-style: none;
  padding: 1rem 0;
  margin: 0;
  flex: 1;
}

.sidebar__nav-item {
  margin: 0.25rem 0.75rem;
}

.sidebar__nav-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 0.875rem;
  border-radius: 8px;
  color: rgba(255, 255, 255, 0.75);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  transition:
    background var(--transition-speed),
    color var(--transition-speed);
  white-space: nowrap;
  overflow: hidden;
}

.sidebar__nav-link:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.sidebar__nav-link--active {
  background: var(--color-primary-light);
  color: #fff;
}

.sidebar__nav-link:focus-visible {
  outline: 2px solid #fff;
  outline-offset: 2px;
}

.sidebar__nav-icon {
  font-size: 1.1rem;
  flex-shrink: 0;
  width: 1.25rem;
  text-align: center;
}

.sidebar__nav-label {
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar__footer {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 1rem 0.75rem;
}

.sidebar__logout {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 0.875rem;
  border-radius: 8px;
  color: rgba(255, 255, 255, 0.65);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
  transition:
    background var(--transition-speed),
    color var(--transition-speed);
  white-space: nowrap;
  overflow: hidden;
}

.sidebar__logout:hover {
  background: rgba(255, 100, 100, 0.2);
  color: #ff9999;
}

.sidebar__logout:focus-visible {
  outline: 2px solid #fff;
  outline-offset: 2px;
}
</style>
