<script setup lang="ts">

</script>

<template>
  <section :class="$style.section">
    <div :class="$style.inner">
      <h1 :class="$style.title">
        Архитектура
      </h1>
      <p :class="$style.lead" />
    </div>
  </section>
</template>

<style module lang="scss">
@keyframes flowLine {
  0% {
    transform: translateY(-30px);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    transform: translateY(300%);
    opacity: 0;
  }
}

.inner {
  max-width: min(1000px, 95%);
  margin: 0 auto;
  padding: 0 var(--space-md);
  box-sizing: border-box;
}

.section {
  min-height: 100vh;
  width: 100vw;
  padding-top: calc(var(--header-height) + var(--section-padding));
  position: relative;
  overflow: hidden;
}

.title {
  font-size: var(--font-size-2xl);
  color: var(--color-primary);
  margin-bottom: var(--space-lg);
  text-align: center;
  position: relative;
  z-index: 1;
}

.lead {
  max-width: min(800px, 95%);
  margin: 0 auto var(--space-xl);
  color: var(--color-secondary);
  font-size: var(--font-size-lg);
  line-height: 1.6;
  text-align: center;
  position: relative;
}

.step {
  display: grid;
  grid-template-columns: auto 1fr;
  grid-template-areas: 'left right';
  align-items: start;
  gap: var(--space-lg);

  @media (max-width: 768px) {
    grid-template-columns: 1fr;
    grid-template-areas:
      'right'
      'left';
    gap: var(--space-md);
    text-align: center;
  }
}

.flow {
  max-width: min(1000px, 95%);
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: var(--space-sm);
  position: relative;
  z-index: 2;
}

.left {
  grid-area: left;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-sm);
}

.block {
  width: 180px;
  height: 60px;
  position: relative;
  perspective: 1000px;

  &Inner {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: var(--space-md);
    background: linear-gradient(145deg, #ffffff, #f0f9ff);
    border-radius: 10px;
    box-shadow:
      0 4px 20px rgba(99, 102, 241, 0.1),
      0 0 0 1px rgba(209, 213, 219, 0.5);
    padding: 0 20px;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    z-index: 2;
    overflow: hidden;
    font-weight: 600;
    color: var(--color-primary);
  }
}

.arrow {
  height: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  margin: 5px 0;
}

.arrowLine {
  width: 4px;
  height: 100%;
  background: linear-gradient(to bottom, #93c5fd, #bfdbfe);
  position: relative;
  overflow: hidden;
  border-radius: 4px;
}

.arrowLine::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: calc(20px + 14px);
  background: linear-gradient(to bottom, #3b82f6, #60a5fa);
  animation: flowLine 1.5s infinite ease-in-out;
}

.arrowHead {
  width: 14px;
  height: 14px;
  background: #3b82f6;
  clip-path: polygon(50% 0%, 100% 100%, 0% 100%);
  border-radius: 2px;
  transform: rotate(180deg);
}

.right {
  grid-area: right;
  font-size: var(--font-size-base);
  color: var(--color-secondary);
  line-height: 1.6;
  text-align: left;
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(4px);
  border-radius: 12px;
  padding: var(--space-md);
  border: 1px solid rgba(209, 213, 219, 0.5);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.03);
  transition: all 0.3s ease;

  @media (max-width: 768px) {
    text-align: center;
  }
}
</style>
