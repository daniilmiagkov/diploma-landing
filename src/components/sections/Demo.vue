<script setup lang="ts">
import Text from '../elements/Text.vue'
import Section from '../Section.vue'

const features = [
  'Видео с оверлеем — цветной кадр + контуры объектов. Разным цветом подсвечиваются объекты, найденные на карте глубины и на цветном изображении',
  'Список объектов — каждый элемент содержит id, диагональ, координаты, номер кадра и счётчик обновлений; по клику раскрывается история состояний (кадр, время, x, y, d).',
  'Гистограмма фракций — динамическое распределение размеров (Chart.js), обновляется при каждом кадре и служит быстрой диагностикой смещений в фракциях.',
  'Плеер управления — кнопки связи с сервером, инициализации камеры, запуска видео, паузы и остановки. Ползунок для выбора кадра.',
]

const videoTips = [
  { top: '20%', left: '15%', text: 'Кадры с оверлеем' },
  { top: '60%', left: '10%', text: 'Список объектов' },
  { top: '70%', left: '70%', text: 'Гистограмма фракций' },
  { top: '43%', left: '15%', text: 'Плеер управления' },

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
        start="top 60%"
      />
      <ol :class="$style.list">
        <Text
          v-for="(feature, i) in features"
          :key="i"
          as="li"
          :text="feature"
          ease="circ"
          start="top 70%"
          :class="$style.feature"
        />
      </ol>
    </Section>

    <div :class="$style.videoContainer">
      <video
        src="/prototype.mp4"
        :class="$style.video"
        muted
        controls
      />

      <div
        v-for="(tip, i) in videoTips"
        :key="i"
        :class="$style.tip"
        :style="{ top: tip.top, left: tip.left }"
      >
        <span :class="$style.number">{{ i + 1 }}</span>
        <span :class="$style.tooltip">{{ tip.text }}</span>
      </div>
    </div>
  </div>
</template>

<style module lang="scss">
.section {
  min-height: fit-content;
}

.videoContainer {
  position: relative;
  width: 100%;
  display: flex;
  justify-content: center;
  margin-top: var(--space-lg);

  .video {
    display: block;
  }

  .tip {
    position: absolute;
    cursor: pointer;
    transform: translate(-50%, -50%);
  }

  .number {
    display: inline-block;
    background: #ff5722;
    color: #fff;
    border-radius: 50%;
    width: 28px;
    height: 28px;
    text-align: center;
    line-height: 28px;
    font-weight: bold;
    font-size: 14px;
    z-index: 10;
  }

  .tooltip {
    visibility: hidden;
    opacity: 0;
    position: absolute;
    top: -40px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0, 0, 0, 0.85);
    color: #fff;
    padding: 6px 10px;
    border-radius: 6px;
    white-space: nowrap;
    font-size: 13px;
    transition:
      opacity 0.2s ease,
      visibility 0.2s ease;
    pointer-events: none;
    z-index: 20;
  }

  .tip:hover .tooltip {
    visibility: visible;
    opacity: 1;
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
