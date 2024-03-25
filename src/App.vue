<template>
  <body>
    <MenSection :showNextSection="showNextSection" v-if="currentSection === 'men-section'" />
    <WomenSection
      :showNextSection="showNextSection"
      v-else-if="currentSection === 'women-section'"
      :circleClasses="circleClasses(womenClothes?.value?.rating)"
    />
    <UnavailableSection :showNextSection="showNextSection" v-else />
  </body>
  <!-- <RouterView /> -->
</template>

<script setup>
// import { RouterLink, RouterView } from 'vue-router'
import UnavailableSection from '@/components/UnavailableSection.vue'
import MenSection from '@/components/MenSection.vue'
import WomenSection from '@/components/WomenSection.vue'

import { ref, computed } from 'vue'

const pageSection = ref(['men-section', 'women-section', 'unavailable-section'])
const currentIndex = ref(0)

const currentSection = computed(() => {
  return pageSection.value[currentIndex.value]
})

const showNextSection = () => {
  currentIndex.value = (currentIndex.value + 1) % pageSection.value.length
}

const circleClasses = (rating) => {
  const classes = []
  const roundedRating = Math.round(rating)
  console.log(roundedRating)
  console.log(roundedRating)
  for (let i = 0; i < 5; i++) {
    classes.push({ circle: true, active: i < roundedRating })
  }
  return classes
}
</script>
