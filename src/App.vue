<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import AppSidebar from './components/AppSidebar.vue'
import AppHeader from './components/AppHeader.vue'
import DashboardView from './views/DashboardView.vue'

const COLLAPSE_BREAKPOINT = 900
const MOBILE_BREAKPOINT = 640

const sidebarCollapsed = ref(false)
const isMobile = ref(false)

function syncLayout() {
  isMobile.value = window.innerWidth <= MOBILE_BREAKPOINT
  sidebarCollapsed.value = window.innerWidth <= COLLAPSE_BREAKPOINT
}

function toggleSidebar() {
  sidebarCollapsed.value = !sidebarCollapsed.value
}

onMounted(() => {
  syncLayout()
  window.addEventListener('resize', syncLayout)
})

onUnmounted(() => {
  window.removeEventListener('resize', syncLayout)
})
</script>

<template>
  <div
    class="app-layout"
    :class="{ 'app-layout--collapsed': sidebarCollapsed }"
  >
    <a href="#main-content" class="skip-link">Ir al contenido principal</a>

    <AppSidebar v-if="!isMobile" :collapsed="sidebarCollapsed" />
    <AppHeader />

    <button
      v-if="!isMobile"
      class="sidebar-toggle"
      @click="toggleSidebar"
      :aria-label="sidebarCollapsed ? 'Expandir menú' : 'Colapsar menú'"
      :aria-expanded="!sidebarCollapsed"
    >
      <span aria-hidden="true">{{ sidebarCollapsed ? '›' : '‹' }}</span>
    </button>
    <DashboardView />

    <footer class="app-footer" role="contentinfo">
      <p>© 2026 Panel Andino · Bogotá, Colombia · Vue 3 + Vite</p>
    </footer>
  </div>
</template>

<style>
/* ─── CSS Custom Properties ─── */
:root {
  --color-primary: #1a472a;
  --color-primary-light: #2d6a4f;
  --color-sidebar-bg: #1a472a;
  --color-bg: #f4f6f4;
  --color-surface: #ffffff;
  --color-border: #e2e8e4;
  --color-text: #1c2b22;
  --color-text-muted: #6b7b72;

  --sidebar-width: 240px;
  --sidebar-collapsed-width: 64px;
  --header-height: 72px;
  --transition-speed: 0.22s;

  --font-base: 'Inter', 'Segoe UI', system-ui, sans-serif;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  font-family: var(--font-base);
  background: var(--color-bg);
  color: var(--color-text);
  -webkit-font-smoothing: antialiased;
}

/* ─── Skip link for keyboard accessibility ─── */
.skip-link {
  position: absolute;
  top: -100%;
  left: 1rem;
  background: var(--color-primary);
  color: #fff;
  padding: 0.5rem 1rem;
  border-radius: 0 0 8px 8px;
  font-weight: 600;
  z-index: 9999;
  transition: top 0.2s;
  text-decoration: none;
}

.skip-link:focus {
  top: 0;
}

/* ─── Main Grid Layout ─── */
.app-layout {
  display: grid;
  grid-template-areas:
    'sidebar header'
    'sidebar main'
    'sidebar footer';
  grid-template-columns: var(--sidebar-width) 1fr;
  grid-template-rows: var(--header-height) 1fr auto;
  min-height: 100vh;
  transition: grid-template-columns var(--transition-speed) ease;
}

.app-layout--collapsed {
  grid-template-columns: var(--sidebar-collapsed-width) 1fr;
}

/* ─── Grid areas ─── */
.app-footer {
  grid-area: footer;
  background: var(--color-surface);
  border-top: 1px solid var(--color-border);
  padding: 1rem 2rem;
  font-size: 0.8rem;
  color: var(--color-text-muted);
  text-align: center;
}

.app-footer p {
  margin: 0;
}

/* ─── Sidebar toggle button (outside grid flow to avoid clipping) ─── */
.sidebar-toggle {
  position: fixed;
  left: calc(var(--sidebar-width) - 14px);
  top: calc(var(--header-height) + 8px);
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--color-primary);
  border: 2px solid #fff;
  color: #fff;
  font-size: 1rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  transition:
    left var(--transition-speed) ease,
    background var(--transition-speed);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
  padding: 0;
  line-height: 1;
}

.app-layout--collapsed .sidebar-toggle {
  left: calc(var(--sidebar-collapsed-width) - 14px);
}

.sidebar-toggle:hover,
.sidebar-toggle:focus-visible {
  background: var(--color-primary-light);
}

.sidebar-toggle:focus-visible {
  outline: 2px solid #fff;
  outline-offset: 2px;
}

/* ─── Responsive: Mobile ─── */
@media (max-width: 640px) {
  .app-layout {
    grid-template-areas:
      'header'
      'main'
      'footer';
    grid-template-columns: 1fr;
    grid-template-rows: var(--header-height) 1fr auto;
  }
}
</style>
