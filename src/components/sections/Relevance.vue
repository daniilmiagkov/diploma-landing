<script setup lang="ts">
</script>

<template>
  <section
    id="relevance"
    data-section="relevance"
    :class="$style.section"
  >
    <h1>
      Актуальность
    </h1>

    <section
      :class="$style.intro"
      lang="ru"
    >
      <p />

      <p />
    </section>
  </section>
</template>

<style module lang="scss">
.section {
  min-height: 100vh;
  padding: var(--section-padding);
  padding-top: calc(var(--header-height) + var(--section-padding));

  h1 {
    font-size: var(--font-size-2xl);
    color: var(--color-primary);
    margin-bottom: var(--space-lg);
    text-align: center;
  }
}

.intro {
  max-width: min(800px, 95%);
  margin: 0 auto var(--space-xl);

  p {
    color: var(--color-secondary);
    line-height: 1.6;
    font-size: var(--font-size-lg);
    margin-bottom: var(--space-md);
    text-align: justify;
    white-space: normal;
    overflow-wrap: anywhere;
    word-wrap: break-word;
    word-break: keep-all;
    hyphens: auto;

    &:last-child {
      margin-bottom: 0;
    }

    padding: 0 var(--space-md);
  }
}
</style>
