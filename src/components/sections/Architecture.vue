<script setup lang="ts">
import { onMounted } from 'vue'
import gsap from 'gsap'
import Section from '../Section.vue'

const list = [
  'Камера глубины ZED2i снимает одновременно цветное изображение и карту глубины.',
  'Сервер (на FastAPI) принимает поток и выполняет предобработку: перевод в градации серого, фильтрацию шумов, бинаризацию.',
  'Алгоритмы сегментации и морфологии «вытаскивают» контуры частиц, а трекер связывает их между кадрами.',
  'Результаты вычислений — координаты, размеры, история движения — передаются клиенту через WebSocket.',
  'На фронтенде (Vue 3) оператор видит кадры с подсвеченными объектами, список частиц, графики распределения и аномалии в реальном времени.',
]

onMounted(() => {
  gsap.context(() => {
    const questions = Array.from(document.querySelectorAll('[data-animation]'))

    questions.forEach((item) => {
      gsap.fromTo(
        item,
        { y: 200, opacity: 0 },
        {
          y: 0,
          opacity: 1,
          ease: 'none',
          scrollTrigger: {
            trigger: item,
            start: 'top bottom',
            markers: true,
            toggleActions: 'play play pause play',
          },
        },
      )
    })
  })
})
</script>

<template>
  <h1
    :class="$style.title"
    data-animation
  >
    Архитектура — как всё устроено внутри
  </h1>
  <Section>
    <p
      :class="$style.content"
      data-animation
    >
      Представьте себе «конвейер данных»:
    </p>
    <ol>
      <li
        v-for="item in list"
        :key="item"
        :class="$style.item"
        data-animation
      >
        {{ item }}
      </li>
    </ol>
  </Section>
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
  white-space: normal;
  overflow-wrap: anywhere;
  word-wrap: break-word;
  word-break: keep-all;
  font-weight: 300;
}

ol {
  list-style: none;
  counter-reset: step;
  padding: 0;
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
</style>
