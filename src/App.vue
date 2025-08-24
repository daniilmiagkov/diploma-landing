<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollSmoother, ScrollToPlugin, ScrollTrigger } from 'gsap/all'
import ArchitectureSection from './components/sections/Architecture.vue'
import DemoSection from './components/sections/Demo.vue'
import RelevanceSection from './components/sections/Relevance.vue'
import Header from './Header.vue'

// @todo: add prefers-reduced-motion

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin, ScrollSmoother)

const styles = useCssModule()
let ctx: gsap.Context
let smoother: globalThis.ScrollSmoother | null = null
const scrollTagRef = ref<HTMLElement | null>(null)

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
          smoother.scrollTo(target, { offset: 0, duration: 1 })
        }
        else {
          gsap.to(window, { duration: 1, scrollTo: { y: target, autoKill: false } })
        }
      })
    })

    function setActive(link) {
      links.forEach(l => l.classList.remove(styles.active))
      link.classList.add(styles.active)
    }

    ScrollTrigger.refresh()
  })

  window.addEventListener('scroll', hideOnScroll, { passive: true })
})
function hideOnScroll() {
  const el = scrollTagRef.value
  if (!el) 
    return
  if (window.scrollY > 20) {
    el.style.transition = 'opacity 0.5s, transform 0.5s'
    el.style.opacity = '0'
    el.style.transform = 'translateY(12px) translateX(-50%)'
    setTimeout(() => el.style.display = 'none', 1000)
    window.removeEventListener('scroll', hideOnScroll)
  }
}
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
  <div :class="styles.pagesScroll">
    <Header />

    <div id="smooth-wrapper">
      <div id="smooth-content">
        <section
          id="relevance"
          :class="[styles.panel]"
          data-panel
        >
          <div :class="styles.inner">
            <RelevanceSection />
          </div>
        </section>
        <section
          id="architecture"
          :class="[styles.panel]"
          data-panel
        >
          <div :class="styles.inner">
            <ArchitectureSection />
          </div>
        </section>
        <section
          id="demo"
          :class="[styles.panel]"
          data-panel
        >
          <div :class="styles.inner">
            <DemoSection />
          </div>
        </section>
      </div>
    </div>
    <div
      ref="scrollTagRef"
      :class="$style.scrollTag"
      aria-hidden="true"
    >
      <span :class="$style.scrollLabel">Scroll</span>
      <svg
        :class="$style.arrow"
        width="16"
        height="16"
        viewBox="0 0 24 24"
        aria-hidden="true"
      >
        <path
          d="M12 5v14M5 12l7 7 7-7"
          fill="none"
          stroke="currentColor"
          stroke-width="1.6"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </div>
  </div>
</template>

<style module>
.pagesScroll {
  position: relative;
  font-family: Inter, system-ui, sans-serif;
}

html {
  scroll-behavior: smooth;
}

#smooth-wrapper {
  position: relative;
  overflow: hidden;
}

.nav {
  position: fixed;
  top: 14px;
  right: 14px;
  z-index: 60;
  background: rgba(0, 0, 0, 0.55);
  padding: 8px 12px;
  border-radius: 10px;
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  align-items: center;
}
.nav a {
  color: #fff;
  text-decoration: none;
  opacity: 0.7;
  font-size: 14px;
  padding: 4px 6px;
  border-radius: 6px;
}
.active {
  opacity: 1;
  background: rgba(255, 255, 255, 0.08);
}

.panel {
  min-height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 200px;
}

h1 {
  font-size: 2rem;
  margin: 0 0 12px 0;
  font-weight: 500;
}
h2 {
  font-size: 1.5rem;
  margin: 0 0 12px 0;
}
p {
  font-size: 1rem;
  line-height: 1.5;
  margin: 0 0 12px 0;
}

.scrollDown {
  margin-top: 12px;
  opacity: 0.85;
}
.arrow {
  margin-left: 8px;
  display: inline-block;
  transform: translateY(0);
}

@media (min-width: 900px) {
  .nav a {
    font-size: 15px;
  }
}

.scrollTag {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 18px;
  z-index: 140;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 12px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.04);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  color: var(--color-secondary, #e6eef8);
  font-size: 13px;
  font-weight: 500;
  box-shadow: 0 6px 18px rgba(2, 8, 23, 0.12);
  pointer-events: none;
  opacity: 0.98;
  transition:
    opacity 0.28s ease,
    transform 0.28s ease;
}

@keyframes float {
  0%,
  100% {
    transform: translate(-50%, 0);
  }
  50% {
    transform: translate(-50%, -8px);
  }
}

.arrow {
  animation: arrowBounce 1s ease-in-out infinite;
}

@keyframes arrowBounce {
  0%,
  100% {
    transform: translateY(0);
    opacity: 1;
  }
  50% {
    transform: translateY(6px);
    opacity: 0.6;
  }
}

@media (prefers-reduced-motion: reduce) {
  .scrollTag,
  .arrow {
    animation: none !important;
  }
}
</style>
