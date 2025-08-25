<script setup lang="ts">

</script>

<template>
  <section :class="$style.section">
    <slot />
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
</style>
