<template>
  <div
    v-if="$nav.currentPage > 1 && $nav.currentLayout !== 'section' && $nav.currentLayout !== 'api-section'"
    class="absolute top-10 right-10"
  >
    <img src="/rgb-stacked-logo-open-edx-2026.svg" class="h-8" />
  </div>
</template>
