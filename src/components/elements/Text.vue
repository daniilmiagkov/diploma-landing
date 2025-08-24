<script setup lang="ts">
import { onMounted, useId } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/all'

defineProps<{
  as: string
  text: string
}>()

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
        ease: 'back',
        scrollTrigger: {
          trigger: text,
          start: 'center center',
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
