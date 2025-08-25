<script setup lang="ts">
import { onMounted, useId } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/all'

const props = withDefaults(
  defineProps<{
    as: string
    text: string
    ease?: gsap.EaseString | gsap.EaseFunction
    start?: string | number | ScrollTrigger.StartEndFunc
  }>(),
  {
    ease: 'back',
    start: 'top 70%',
  },
)

const id = useId()
gsap.registerEffect(ScrollTrigger)

onMounted(() => {
  gsap.context(() => {
    const text = document.getElementById(id)

    gsap.fromTo(
      text,
      { y: 200, opacity: 0 },
      {
        y: 0,
        opacity: 1,
        ease: props.ease,
        scrollTrigger: {
          trigger: text,
          start: props.start,
          end: '+=100%',
          markers: true,
          toggleActions: 'play play pause play',
        },
      },
    )
  })
})
</script>

<template>
  <component
    :is="as"
    :id="id"
  >
    {{ text }}
  </component>
</template>

<style module lang="scss">

</style>
