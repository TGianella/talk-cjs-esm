<template>
  <!-- Lego-instruction-style progress slider, shown on every slide.
       Wrapped in a Transition so it slides in from the right only when it
       first appears (slide 1 -> 2), not on every navigation while shown. -->
  <Transition name="deck-progress-slide">
    <div v-if="!hideProgress" class="deck-progress">
      <div class="deck-progress-track">
        <div class="deck-progress-fill" :style="{ width: `${percent}%` }" />
        <div class="deck-progress-handle" :style="{ left: `${percent}%` }" />
      </div>
    </div>
  </Transition>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

const { currentPage, total, currentSlideRoute } = useNav()

// Hide on the first and last slides, or opt out with `hideProgress: true`
const hideProgress = computed(
  () =>
    currentPage.value <= 1 ||
    currentPage.value >= total.value ||
    currentSlideRoute.value?.meta?.slide?.frontmatter?.hideProgress === true,
)

// Progress from first to last slide (0% on slide 1, 100% on the last slide)
const percent = computed(() => {
  if (total.value <= 1) return 0
  return ((currentPage.value - 1) / (total.value - 1)) * 100
})
</script>

<style scoped>
.deck-progress {
  position: absolute;
  bottom: 14px;
  left: 40px;
  right: 40px;
  z-index: 50;
  pointer-events: none;
}

/* Slide in from the right on entry — transform only, no fade */
.deck-progress-slide-enter-active {
  transition: transform 500ms ease;
}
.deck-progress-slide-enter-from {
  transform: translateX(calc(100% + 40px));
}
.deck-progress-slide-enter-to {
  transform: translateX(0);
}

.deck-progress-track {
  position: relative;
  height: 8px;
  border-radius: 9999px;
  /* darker blue than the #d3ecfd slide background */
  background-color: #8bc0ec;
  border: 1px solid #264358;
}

.deck-progress-fill {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  border-radius: 9999px;
  background-color: #2b7fd4;
  transition: width 300ms ease;
}

.deck-progress-handle {
  position: absolute;
  top: 50%;
  width: 20px;
  height: 20px;
  border-radius: 9999px;
  background-color: #1565c0;
  border: 1px solid #264358;
  transform: translate(-50%, -50%);
  transition: left 300ms ease;
}
</style>
