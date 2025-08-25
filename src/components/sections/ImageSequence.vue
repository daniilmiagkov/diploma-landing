<script setup lang="ts">
import { onBeforeUnmount, onMounted, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const img = (path: string) => `${import.meta.env.BASE_URL}${path}`

const processorSteps = [
  { title: 'Нормализованное серое изображение', url: img('images/2.png') },
  { title: 'Результат медианной фильтрации (5×5)', url: img('images/3.png') },
  { title: 'CLAHE: адаптивная коррекция контраста', url: img('images/4.png') },
  { title: 'EqualizeHist: глобальное выравнивание гистограммы', url: img('images/5.png') },
  { title: 'Адаптивная бинаризация', url: img('images/6.png') },
  { title: 'Маска после морфологического открытия', url: img('images/7.png') },
  { title: 'Карта расстояний (distance transform)', url: img('images/9.png') },
  { title: 'Бинарная маска переднего плана (foreground_mask)', url: img('images/10.png') },
  { title: 'Итоговая сегментация с bounding box', url: img('images/11.png') },
]

const styles = useCssModule()
let ctx: gsap.Context | null = null

function waitForImages(container: HTMLElement) {
  const imgs = Array.from(container.querySelectorAll<HTMLImageElement>('img'))
  const promises = imgs.map((img) => {
    return new Promise<void>((resolve) => {
      if (img.complete && img.naturalWidth !== 0) {
        resolve()
      }
      else {
        img.addEventListener('load', () => resolve(), { once: true })
        img.addEventListener('error', () => resolve(), { once: true })
      }
    })
  })
  return Promise.all(promises)
}

onMounted(() => {
  ctx = gsap.context(() => {
    const stackSelector = `.${styles.comparisonStack}`
    const layerSelector = `.${styles.layer}`
    const captionSelector = `.${styles.layerCaption}`

    const stack = document.querySelector<HTMLElement>(stackSelector)
    if (!stack)
      return

    waitForImages(stack).then(() => {
      requestAnimationFrame(() => initGsap(stack, layerSelector, captionSelector))
    })
  })
})

function initGsap(stack: HTMLElement, layerSelector: string, captionSelector: string) {
  const layers = Array.from(stack.querySelectorAll<HTMLElement>(layerSelector))
  if (layers.length <= 1)
    return

  const totalWidth = stack.offsetWidth * (layers.length - 1)

  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: stack,
      start: 'top top',
      end: () => `+=${totalWidth}`,
      scrub: true,
      pin: true,
      anticipatePin: 1,
    },
    defaults: { ease: 'none' },
  })

  for (let i = 1; i < layers.length; i++) {
    const layer = layers[i]
    const img = layer.querySelector<HTMLImageElement>('img')
    const caption = layer.querySelector<HTMLElement>(captionSelector)

    if (!img)
      continue

    gsap.set(layer, { xPercent: 100 })
    gsap.set(img, { xPercent: -100 })
    if (caption) {
      gsap.set(caption, { y: 0, opacity: 0 })
    }

    const pos = i - 1
    tl.to(layer, { xPercent: 0, duration: 1 }, pos)
      .to(img, { xPercent: 0, duration: 1 }, pos)

    if (caption) {
      tl.to(caption, { y: 0, opacity: 1, duration: 0.45, ease: 'power2.out' }, pos + 0.15)
    }
  }

  const baseLayer = layers[0]
  const baseCaption = baseLayer?.querySelector<HTMLElement>(`.${styles.layerCaption}`)
  if (baseCaption) {
    gsap.set(baseCaption, { y: 0, opacity: 1 })
  }

  ScrollTrigger.refresh()
}

onBeforeUnmount(() => {
  if (ctx) {
    ctx.revert()
    ctx = null
  }
  ScrollTrigger.getAll().forEach(t => t.kill())
})
</script>

<template>
  <div :class="$style.section">
    <h1
      :class="$style.title"
      data-animation
    >
      Пример обработки кадра — как это происходит шаг за шагом
    </h1>

    <section :class="$style.comparisonStack">
      <div
        :class="[$style.layer, $style.base]"
        :style="{ zIndex: 1 }"
      >
        <img
          :src="processorSteps[0].url"
          :alt="`${processorSteps[0].title} base`"
          loading="eager"
        >
        <div :class="$style.layerCaption">
          {{ processorSteps[0].title }}
        </div>
      </div>

      <template
        v-for="(item, idx) in processorSteps.slice(1)"
        :key="idx"
      >
        <div
          :class="$style.layer"
          :style="{ zIndex: idx + 2 }"
          aria-hidden="true"
        >
          <img
            :src="item.url"
            :alt="item.title"
            loading="lazy"
          >
          <div :class="$style.layerCaption">
            {{ item.title }}
          </div>
        </div>
      </template>
    </section>
  </div>
</template>

<style module lang="scss">
.title {
  line-height: 1.15;
  margin-bottom: var(--space-4xl);
}

.content,
.item {
  color: var(--color-secondary);
  font-size: var(--font-size-lg);
  margin-bottom: var(--space-xs);
  text-align: justify;
  font-weight: 300;
}

.item {
  counter-increment: step;
  position: relative;
  margin-bottom: var(--space-xs);
  padding-left: var(--space-xl);
}

.item::before {
  content: counter(step, decimal-leading-zero) '.';
  position: absolute;
  left: 0;
  top: 0;
  text-align: right;
  font-variant-numeric: tabular-nums;
}

.panel {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 90vh;
  top: 10px;
}

.section {
  min-height: 100vh;
}

.comparisonStack {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%;
  overflow: hidden;
  background: #111;
}

.base {
  transform: none;
}

.layer {
  position: absolute;
  inset: 0;
  overflow: hidden;
  will-change: transform;
}

.layer img {
  position: absolute;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.layerCaption {
  position: absolute;
  left: 16px;
  bottom: 16px;
  z-index: 30;
  max-width: calc(100% - 40px);
  background: rgba(0, 0, 0, 0.8);
  color: #fff;
  padding: 8px 12px;
  border-radius: 8px;
  font-size: 14px;
  line-height: 1.2;
  opacity: 0;
  transform: translateY(20px);
  pointer-events: none;
}

.num {
  font-weight: 700;
  width: 18px;
  text-align: right;
  opacity: 0.9;
}

.txt {
  opacity: 0.95;
}

@media (max-width: 768px) {
  :global(body) {
    height: 220vh;
  }
  .caption {
    right: 8px;
    bottom: 8px;
    font-size: 11px;
    padding: 6px 8px;
  }
  .layerCaption {
    left: 10px;
    bottom: 10px;
    font-size: 13px;
    padding: 6px 10px;
  }
}
</style>
