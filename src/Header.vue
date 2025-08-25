<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollSmoother, ScrollToPlugin, ScrollTrigger } from 'gsap/all'
import { Menu, X } from 'lucide-vue-next'

const items = [
  { text: 'Актуальность', href: '#relevance' },
  { text: 'Архитектура', href: '#architecture' },
  { text: 'Демо', href: '#demo' },
]

const isMenuOpen = ref(false)
const toggleMenu = () => (isMenuOpen.value = !isMenuOpen.value)

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin, ScrollSmoother)

const styles = useCssModule()
let ctx: gsap.Context
let smoother: globalThis.ScrollSmoother | null = null

onMounted(() => {
  ctx = gsap.context(() => {
    try {
      if (typeof ScrollSmoother !== 'undefined' && ScrollSmoother.create) {
        smoother = ScrollSmoother.create({
          wrapper: '#smooth-wrapper',
          content: '#smooth-content',
          smooth: 1.0,
          effects: true,
        })
        console.log('ScrollSmoother initialized')
      }
      else {
        console.warn('ScrollSmoother not available — fallback to native/GSAP scroll')
      }
    }
    catch (err) {
      console.warn('Failed to init ScrollSmoother:', err)
      smoother = null
    }

    const links = Array.from(document.querySelectorAll('nav [data-nav-link]'))
    links.forEach((a) => {
      const href = a.getAttribute('href')
      if (!href)
        return
      const target = document.querySelector(href)
      if (!target)
        return

      ScrollTrigger.create({
        trigger: target,
        start: 'top center',
        end: 'bottom center',
        onEnter: () => setActive(a),
        onEnterBack: () => setActive(a),
      })

      a.addEventListener('click', (e) => {
        e.preventDefault()
        if (smoother && typeof smoother.scrollTo === 'function') {
          smoother.scrollTo(target, true, 'top')
        }
        else {
          gsap.to(window, { duration: 1, scrollTo: { y: target, autoKill: false } })
        }
      })
    })

    function setActive(link: Element) {
      links.forEach(l => l.classList.remove(styles.active))
      link.classList.add(styles.active)
    }

    ScrollTrigger.refresh()
  })
})

onBeforeUnmount(() => {
  if (ctx)
    ctx.revert()
  try {
    if (smoother && typeof smoother.kill === 'function') {
      smoother.kill()
      smoother = null
    }
  }
  catch (e) {
    console.warn('Error while killing smoother', e)
  }
  ScrollTrigger.getAll().forEach(st => st.kill())
})
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
  left: 0;
  right: 0;
  z-index: 9999;
  height: var(--header-height);
  display: flex;
  align-items: center;
  pointer-events: auto;
}

.nav {
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  z-index: 0;
  border-radius: 9999px;
  background: color-mix(in srgb, var(--color-background), #ffffff00 5%);
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
  padding: 0 var(--space-xs);
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
  color: var(--color-accent);
}
</style>
