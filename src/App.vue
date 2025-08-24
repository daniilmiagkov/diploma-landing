<script setup lang="ts">
import { onMounted, ref, useCssModule } from 'vue'
import Text from './components/elements/Text.vue'
import ArchitectureSection from './components/sections/Architecture.vue'
import DemoSection from './components/sections/Demo.vue'
import RelevanceSection from './components/sections/Relevance.vue'
import Header from './Header.vue'
// @todo: add prefers-reduced-motion

const styles = useCssModule()
const scrollTagRef = ref<HTMLElement | null>(null)

onMounted(() => {
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
        <Text
          as="h1"
          text="А теперь подробнее!"
          :class="$style.mainNotification"
        />
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

.panel {
  min-height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 200px;
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

.mainNotification {
  margin-bottom: 300px;
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
