<script setup lang="ts">
import { onMounted, useCssModule } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/all'
import Section from '../Section.vue'

type QA = {
  id: string
  question: string
  answer: string
}

const styles = useCssModule()
gsap.registerPlugin(ScrollTrigger)
const faqs: QA[] = [
  {
    id: 'for-whom',
    question: 'Для кого проект?',
    answer:
      'Для инженеров и операторов производственных линий, специалистов контроля качества, лабораторий анализа материалов и технических команд карьеров и дробильно-сортировочных комплексов. Полезно менеджерам и аналитикам, которым нужны количественные данные о фракционном составе сырья.',
  },
  {
    id: 'purpose',
    question: 'Для чего нужен проект?',
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
    question: 'Как проект работает в двух словах?',
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
    question: 'Чем проект лучше ручного контроля и когда выгодно?',
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
    question: 'Какие промышленные задачи решает проект?',
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

onMounted(() => {
  gsap.context(() => {
    function allSettings(trigger: any, start: string): gsap.TweenVars {
      return {
        x: 0,
        opacity: 1,
        ease: 'power1.out',
        scrollTrigger: {
          trigger,
          start,
          end: '+=100%',
          toggleActions: 'play none none none',
        },
      }
    }

    const questions = Array.from(document.getElementsByClassName(styles.question))

    questions.forEach((question) => {
      gsap.fromTo(
        question,
        { x: -200, opacity: 0 },
        allSettings(question, 'center bottom'),
      )
    })

    const answers = Array.from(document.querySelectorAll('[data-qa="answer"]'))
    answers.forEach((answer) => {
      gsap.fromTo(
        answer,
        { x: 200, opacity: 0 },
        allSettings(answer, 'center bottom'),
      )
    })
  })
})
</script>

<template>
  <Section>
    <div :class="$style.container">
      <template
        v-for="(item) in faqs"
        :key="item.id"
      >
        <h2
          :id="item.id"
          :class="$style.question"
        >
          {{ item.question }}
        </h2>
        <p data-qa="answer">
          {{ item.answer }}
        </p>
      </template>
    </div>
  </Section>
</template>

<style module lang="scss">
.container {
  padding: 0 var(--space-md);
}

.question {
  font-size: var(--font-size-2xl);
  color: var(--color-primary);
  margin-top: var(--space-8xl);
  margin-bottom: var(--space-2xl);
  text-align: center;
  line-height: 1.15;
}
</style>
