<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollSmoother, ScrollToPlugin, ScrollTrigger } from 'gsap/all'

const items = [
  { text: 'Актуальность', href: '#relevance' },
  { text: 'Архитектура', href: '#architecture' },
  { text: 'Прототип', href: '#demo' },
]

const isMenuOpen = ref(false)

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
    <a
      href="https://daniilmiagkov.github.io/diploma-landing/"
      :class="$style.linkOnGithub"
      target="_blank"
    >
      daniilmiagkov/diploma-landing
    </a>
    <nav
      :class="$style.nav"
      aria-label="Главная навигация"
    >
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
  top: var(--space-xs);
  left: 0;
  right: 0;
  z-index: 9999;
  height: var(--header-height);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  align-items: center;
}

.nav {
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  z-index: 0;
  border-radius: 9999px;
  background: color-mix(in srgb, var(--color-gray), #ffffff00 15%);
}

.list {
  display: flex;
  margin: 0;
  list-style: none;
  align-items: center;
}

.link,
.linkOnGithub {
  display: block;
  padding: var(--space-xs) var(--space-md);
  font-size: var(--font-size-sm);
  font-weight: 500;
  width: 160px;
  color: var(--color-primary);
  transition: color var(--transition-fast);
  white-space: nowrap;
  text-decoration: none;
  text-align: center;
}

.link:hover {
  color: var(--color-primary);
}

.linkOnGithub:hover {
  color: var(--color-accent);
}

.active {
  opacity: 1;
  color: var(--color-gray);
  background-color: var(--color-primary);
  border-radius: 9999px;

  &:hover {
    color: var(--color-gray);
  }
}
</style>
