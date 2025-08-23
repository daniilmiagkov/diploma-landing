<script setup>
import { onBeforeUnmount, onMounted, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollSmoother, ScrollToPlugin, ScrollTrigger } from 'gsap/all'
import ArchitectureSection from './components/sections/Architecture.vue'
import DemoSection from './components/sections/Demo.vue'
import RelevanceSection from './components/sections/Relevance.vue'
import Header from './Header.vue'

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin, ScrollSmoother)

const styles = useCssModule()
let ctx
let smoother = null

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
</style>
