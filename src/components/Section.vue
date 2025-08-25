<script setup lang="ts">

</script>

<template>
  <section :class="$style.section">
    <slot />
  </section>
</template>

<style module lang="scss">
.section {
  padding: var(--section-padding);
  max-width: min(800px, 95%);
  margin: 0 auto var(--space-xl);
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
  font-weight: 300;
}
</style>
