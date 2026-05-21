<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
const notificationCount = ref(3)
const showNotifications = ref(false)

const notifications = [
  { id: 1, text: 'Nuevo convenio en la regional Antioquia', time: 'hace 5 min' },
  { id: 2, text: 'Mateo Vásquez cerró la integración DIAN', time: 'hace 1 h' },
  { id: 3, text: 'Reunión de equipo mañana 8:00 (hora Bogotá)', time: 'hace 3 h' },
]

function toggleNotifications() {
  showNotifications.value = !showNotifications.value
}

function closeNotifications() {
  showNotifications.value = false
}
</script>

<template>
  <header class="header" role="banner">
    <div class="header__search">
      <label for="global-search" class="sr-only">Buscar</label>
      <span class="header__search-icon" aria-hidden="true">🔍</span>
      <input
        id="global-search"
        v-model="searchQuery"
        type="search"
        class="header__search-input"
        placeholder="Buscar proyectos, regiones, responsables..."
        aria-label="Buscar en el sistema"
      />
    </div>

    <div class="header__actions">
      <div class="header__notifications" v-click-outside="closeNotifications">
        <button
          class="header__icon-btn"
          @click="toggleNotifications"
          :aria-expanded="showNotifications"
          aria-haspopup="true"
          aria-label="`${notificationCount} notificaciones sin leer`"
        >
          <span aria-hidden="true">🔔</span>
          <span v-if="notificationCount > 0" class="header__badge" aria-hidden="true">
            {{ notificationCount }}
          </span>
        </button>

        <div
          v-if="showNotifications"
          class="header__dropdown"
          role="dialog"
          aria-label="Notificaciones"
        >
          <p class="header__dropdown-title">Notificaciones</p>
          <ul role="list" class="header__notif-list">
            <li
              v-for="notif in notifications"
              :key="notif.id"
              class="header__notif-item"
            >
              <span class="header__notif-dot" aria-hidden="true"></span>
              <div>
                <p class="header__notif-text">{{ notif.text }}</p>
                <p class="header__notif-time">{{ notif.time }}</p>
              </div>
            </li>
          </ul>
        </div>
      </div>

      <div class="header__user" role="group" aria-label="Perfil de usuario">
        <div class="header__avatar" aria-hidden="true">JD</div>
        <div class="header__user-info">
          <span class="header__user-name">Joiner Davila</span>
          <span class="header__user-role">Coordinador nacional</span>
        </div>
      </div>
    </div>
  </header>
</template>

<style scoped>
.header {
  grid-area: header;
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 1.5rem;
  height: 72px;
  gap: 1rem;
  position: sticky;
  top: 0;
  z-index: 50;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
}

.header__search {
  position: relative;
  flex: 1;
  max-width: 400px;
}

.header__search-icon {
  position: absolute;
  left: 0.75rem;
  top: 50%;
  transform: translateY(-50%);
  font-size: 0.9rem;
  pointer-events: none;
}

.header__search-input {
  width: 100%;
  padding: 0.5rem 1rem 0.5rem 2.25rem;
  border: 1px solid var(--color-border);
  border-radius: 8px;
  font-size: 0.875rem;
  background: var(--color-bg);
  color: var(--color-text);
  transition: border-color var(--transition-speed);
  box-sizing: border-box;
}

.header__search-input:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(45, 106, 79, 0.15);
}

.header__search-input::placeholder {
  color: var(--color-text-muted);
}

.header__actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.header__notifications {
  position: relative;
}

.header__icon-btn {
  position: relative;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.25rem;
  padding: 0.5rem;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background var(--transition-speed);
  color: var(--color-text);
}

.header__icon-btn:hover {
  background: var(--color-bg);
}

.header__icon-btn:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

.header__badge {
  position: absolute;
  top: 2px;
  right: 2px;
  background: #e53935;
  color: #fff;
  font-size: 0.625rem;
  font-weight: 700;
  min-width: 16px;
  height: 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 3px;
}

.header__dropdown {
  position: absolute;
  top: calc(100% + 0.5rem);
  right: 0;
  width: 280px;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
  z-index: 200;
  padding: 0.75rem 0;
  animation: fadeIn 0.15s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-6px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header__dropdown-title {
  font-weight: 600;
  font-size: 0.875rem;
  color: var(--color-text);
  padding: 0 1rem 0.5rem;
  margin: 0;
  border-bottom: 1px solid var(--color-border);
}

.header__notif-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.header__notif-item {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  transition: background var(--transition-speed);
}

.header__notif-item:hover {
  background: var(--color-bg);
}

.header__notif-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--color-primary);
  flex-shrink: 0;
  margin-top: 5px;
}

.header__notif-text {
  margin: 0;
  font-size: 0.825rem;
  color: var(--color-text);
}

.header__notif-time {
  margin: 0.2rem 0 0;
  font-size: 0.75rem;
  color: var(--color-text-muted);
}

.header__user {
  display: flex;
  align-items: center;
  gap: 0.625rem;
  cursor: default;
}

.header__avatar {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: var(--color-primary);
  color: #fff;
  font-size: 0.75rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.header__user-info {
  display: flex;
  flex-direction: column;
  line-height: 1.3;
}

.header__user-name {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--color-text);
}

.header__user-role {
  font-size: 0.75rem;
  color: var(--color-text-muted);
}

@media (max-width: 600px) {
  .header__user-info {
    display: none;
  }

  .header__search {
    max-width: none;
  }
}
</style>
