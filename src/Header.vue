<script setup lang="ts">
import { ref } from 'vue'
import { Menu, X } from 'lucide-vue-next'

const items = [
  { text: 'Актуальность', href: '#relevance' },
  { text: 'Архитектура', href: '#architecture' },
  { text: 'Демо', href: '#demo' },
]

const isMenuOpen = ref(false)
const toggleMenu = () => (isMenuOpen.value = !isMenuOpen.value)
</script>

<template>
  <header :class="$style.header">
    <nav
      :class="$style.nav"
      aria-label="Главная навигация"
    >
      <button
        type="button"
        :class="$style.burger"
        aria-label="Меню"
        :aria-expanded="isMenuOpen"
        @click="toggleMenu"
      >
        <transition
          name="icon-fade"
          mode="out-in"
        >
          <Menu
            v-if="!isMenuOpen"
            key="menu"
            :size="24"
          />
          <X
            v-else
            key="close"
            :size="24"
          />
        </transition>
      </button>

      <ul :class="[$style.list, isMenuOpen && $style.open]">
        <li
          v-for="item in items"
          :key="item.href"
          :class="$style.item"
          @click="isMenuOpen = false"
        >
          <a
            :href="item.href"
            :class="$style.link"
            data-nav-link
          >
            {{ item.text }}
          </a>
        </li>
      </ul>
    </nav>
  </header>
</template>

<style module lang="scss">
.header {
  position: fixed;
  top: 8px;
  left: 0;
  right: 0;
  z-index: 9999;
  padding: 0 var(--container-padding);
  height: var(--header-height);
  display: flex;
  align-items: center;
  pointer-events: auto;
}

.nav {
  width: min(1200px, 100%);
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  z-index: 0;
  // background: rgba(255, 255, 255, 0.85);
  padding: 0 5%;
  border-radius: 9999px;
  backdrop-filter: blur(6px);
  // box-shadow: 0 6px 20px rgba(0, 0, 0, 0.06);
}

@media (min-width: 641px) {
  .nav {
    justify-content: center;
  }
  .header {
    justify-content: center;
  }
}

.burger {
  display: none;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  color: black;
  @media (max-width: 640px) {
    display: block;
  }
  position: relative;
  z-index: 30;
  -webkit-tap-highlight-color: transparent;
  -webkit-appearance: none;
  appearance: none;
}

.list {
  display: flex;
  gap: var(--space-md);
  margin: 0;
  padding: 10px;
  list-style: none;
  align-items: center;

  @media (max-width: 640px) {
    position: absolute;
    top: var(--header-height);
    left: 0;
    right: 0;
    background: white;
    flex-direction: column;
    align-items: center;
    gap: var(--space-md);
    padding: var(--space-md);
    transform: translateY(-120%);
    transition: transform 0.34s cubic-bezier(0.2, 0.9, 0.3, 1);
    z-index: 60;
    will-change: transform;
    box-shadow: 0 6px 24px rgba(0, 0, 0, 0.08);
    border-bottom-left-radius: 12px;
    border-bottom-right-radius: 12px;
  }
}

.open {
  @media (max-width: 640px) {
    transform: translateY(0);
  }
}

.item {
  position: relative;
}

.link {
  display: block;
  padding: var(--space-xs) var(--space-sm);
  font-size: var(--font-size-sm);
  font-weight: 500;
  color: var(--color-secondary);
  transition: color var(--transition-fast);
  white-space: nowrap;
  text-decoration: none;
}
.link:hover {
  color: var(--color-accent);
}

.active {
  opacity: 1;
  &::after {
    content: '';
    position: absolute;
    bottom: calc(-1 * var(--space-xs));
    left: 0;
    right: 0;
    height: 2px;
    background: var(--color-accent);
  }
}
</style>
