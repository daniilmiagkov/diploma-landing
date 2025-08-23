<script setup lang="ts">
import { onMounted, TriggerOpTypes, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/all'

type QA = {
  id: string
  question: string
  answer: string
}

const styles = useCssModule()
gsap.registerEffect(ScrollTrigger)
const faqs: QA[] = [
  {
    id: 'for-whom',
    question: 'Для кого это?',
    answer:
      'Для инженеров и операторов производственных линий, специалистов контроля качества, лабораторий анализа материалов и технических команд карьеров и дробильно-сортировочных комплексов. Полезно менеджерам и аналитикам, которым нужны количественные данные о фракционном составе сырья.',
  },
  {
    id: 'purpose',
    question: 'Для чего это нужно?',
    answer:
      'Автоматически измерять и отслеживать распределение размеров частиц в реальном времени: своевременно выявлять отклонения, диагностировать износ оборудования, обнаруживать засоры и контролировать дозирование.',
  },
  {
    id: 'result',
    question: 'Что в итоге получает предприятие?',
    answer:
      'Непрерывный источник достоверных данных о фракциях, оперативные оповещения, визуализация потока и история объектов — инструменты для обоснованных технологических решений и сокращения эксплуатационных расходов.',
  },
  {
    id: 'how',
    question: 'Как это работает в двух словах?',
    answer:
      'Камера глубины снимает цвет и карту глубины → сервер предобрабатывает кадры → алгоритмы сегментируют и измеряют частицы → трекинг связывает объекты между кадрами → результаты отображаются в веб-интерфейсе.',
  },
  {
    id: 'data',
    question: 'Какие данные используются?',
    answer:
      'Цветные изображения и карты глубины (например, SVO с ZED2i). Глубина позволяет переводить пиксели в миллиметры для точного учёта фракций.',
  },
  {
    id: 'vs-manual',
    question: 'Чем это лучше ручного контроля и когда выгодно?',
    answer:
      'Система даёт воспроизводимые количественные измерения и стабильную оценку при больших объёмах данных; особенно эффективна при непрерывных потоках и высоких скоростях производства.',
  },
  {
    id: 'hardware',
    question: 'Нужны ли мощные GPU и нейросети?',
    answer:
      'Нет — решение основано на классических методах обработки изображений (фильтрация, бинаризация, морфология, контурный анализ, трекинг) и работает в реальном времени без мощного железа.',
  },
  {
    id: 'industrial-tasks',
    question: 'Какие промышленные задачи решает система?',
    answer:
      'Контроль фракционного состава на конвейерах и в бункерах, ранняя диагностика износа дробилок и сепараторов, обнаружение засоров, мониторинг дозирования и контроль пыления на площадке.',
  },
  {
    id: 'benefit',
    question: 'Какая коммерческая выгода?',
    answer:
      'Снижение брака и переработок, уменьшение простоев и ремонтных работ, экономия на лабораторных тестах и повышение общей эффективности производства.',
  },
]
// eslint-disable-next-line unused-imports/no-unused-vars
let ctx: gsap.Context | null = null
onMounted(() => {
  ctx = gsap.context(() => {
    function allSettings(trigger: any, start: string): gsap.TweenVars {
      return {
        x: 0,
        opacity: 1,
        ease: 'power1.out',
        duration: 1,

        scrollTrigger: {
          trigger,
          start,
          end: '+=100%',
          markers: true,
          toggleActions: 'restart reverse play reverse',
        },
      }
    }

    const questions = Array.from(document.getElementsByClassName(styles.question))

    questions.forEach((question) => {
      gsap.fromTo(
        question,
        { x: -200, opacity: 0 },
        allSettings(question, 'center center'),
      )
    })

    const answers = Array.from(document.getElementsByClassName(styles.answer))

    answers.forEach((answer) => {
      gsap.fromTo(
        answer,
        { x: 200, opacity: 0 },
        allSettings(answer, 'center center'),
      )
    })
  })
})
</script>

<template>
  <section
    id="relevance"
    data-section="relevance"
    :class="$style.section"
  >
    <div :class="$style.container">
      <template
        v-for="(item) in faqs"
        :key="item.id"
      >
        <h1
          :id="item.id"
          :class="$style.question"
        >
          {{ item.question }}
        </h1>
        <p :class="$style.answer">
          {{ item.answer }}
        </p>
      </template>
    </div>
  </section>
</template>

<style module lang="scss">
.section {
  min-height: 100vh;
  padding: var(--section-padding);
  padding-top: calc(var(--header-height) + var(--section-padding));
  max-width: min(800px, 95%);
  margin: 0 auto var(--space-xl);
}

.container {
  padding: 0 var(--space-md);
}

.question {
  font-size: var(--font-size-2xl);
  color: var(--color-primary);
  margin-top: var(--space-8xl);
  text-align: center;
  line-height: 1.15;
}

.answer {
  color: var(--color-secondary);
  line-height: 1.6;
  font-size: var(--font-size-lg);
  margin-bottom: var(--space-2xl);
  text-align: justify;
  white-space: normal;
  overflow-wrap: anywhere;
  word-wrap: break-word;
  word-break: keep-all;
  hyphens: auto;

  &:last-child {
    margin-bottom: 0;
  }
}
</style>
