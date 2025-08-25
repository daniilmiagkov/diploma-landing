<script setup lang="ts">
import Text from '../elements/Text.vue'
import Section from '../Section.vue'

const features = [
  'Видео с оверлеем — цветной кадр + контуры и ID объектов, размеры в мм рядом с каждым объектом; новые объекты подсвечиваются отдельно, исчезнувшие помечаются как «пропавшие».',
  'Сайдбар со списком объектов — каждый элемент содержит id, диагональ, координаты, время последнего обнаружения и счётчик обновлений; по клику раскрывается история состояний (кадр, время, x, y, d).',
  'Гистограмма фракций — динамическое распределение размеров (Chart.js), обновляется при каждом кадре и служит быстрой диагностикой смещений в фракциях.',
  'Плеер управления — кнопки Init Camera, Play / Pause / Stop, ползунок для выбора диапазона кадров и кнопка «Play range» для последовательного воспроизведения.',
  'Статусы соединения и камеры — индикаторы WebSocket (готов/нет), состояние инициализации камеры и сообщения об ошибках.',
]
</script>

<template>
  <div>
    <Text
      as="h1"
      text="Прототип!"
    />
    <Section :class="$style.section">
      <Text
        as="p"
        text="Прототип интерфейса — это рабочая панель оператора для интерактивного контроля и анализа видеопотока в реальном времени. Основная идея — дать инженеру быстрый и однозначный доступ к важным данным без лишних кликов."
        ease="circ"
        start="top 50%"
        :class="$style.intro"
      />
      <Text
        as="h2"
        text="Что на экране"
        ease="circ"
        start="top 50%"
      />
      <ol :class="$style.list">
        <Text
          v-for="(feature, i) in features"
          :key="i"
          as="li"
          :text="feature"
          ease="circ"
          start="center 60%"
          :class="$style.feature"
        />
      </ol>
    </Section>
    <div :class="$style.videoContainer">
      <video
        src="/public/prototype.mp4"
        :class="$style.video"
        muted
        controls
      />
    </div>
  </div>
</template>

<style module lang="scss">
.section {
  min-height: fit-content;
}

.videoContainer {
  width: 100%;
  align-items: center;
  flex-direction: column;
  display: flex;

  .video {
    margin: 0 auto;
  }
}

.list {
  list-style: none;
  counter-reset: step;
  padding: 0;
}

.feature {
  counter-increment: step;
  position: relative;
  margin-bottom: var(--space-xs);
  padding-left: var(--space-xl);
}

.feature::before {
  content: counter(step, decimal-leading-zero) '.';
  position: absolute;
  left: 0;
  top: 0;
  text-align: right;
  font-variant-numeric: tabular-nums;
}

.intro {
  margin-bottom: var(--space-xl);
}
</style>
