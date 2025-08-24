<script setup lang="ts">
import { onMounted } from 'vue'
import gsap from 'gsap'
import Section from '../Section.vue'

const list = [
  'Захват кадра: получаем цветное изображение и карту глубины.',
  'Калибровка глубины: по глубине переводим пиксели в миллиметры (зонд калибровки камеры).',
  'Предобработка: преобразование в серый, медианная фильтрация для удаления импульсных шумов.',
  'Бинаризация: адаптивный порог или порог по контрасту для выделения частиц.',
  'Морфология: закрытие/открытие для заполнения дыр и удаления мелкого мусора.',
  'Поиск компонент: выделение связных компонент, вычисление контуров.',
  'Вычисление метрик: для каждой компоненты — центр, bounding box, площадь, эквивалентная диагональ. По глубине — масштабируем в мм.',
  'Трекинг: ближайшее соответствие с линейной экстраполяцией для связывания объектов между кадрами; нумерация, накопление истории.',
  'Агрегация: расчёт гистограмм фракций, средних значений, выявление отклонений.',
  'Отправка: формирование JSON-пакета и отправка клиенту через WebSocket (контуры + метрики + метки событий).',
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
  <div>
    <h1
      :class="$style.title"
      data-animation
    >
      Пример обработки кадра — как это происходит шаг за шагом
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
