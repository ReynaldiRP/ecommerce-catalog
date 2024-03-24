<template>
  <article :class="checkCardSection()" id="card">
    <slot></slot>
  </article>
</template>
<script setup>
import { ref } from 'vue'

const cardSection = ref('card-section')

const checkCardSection = () => {
  const newValue = cardSection.value

  if (newValue == 'card-section') {
    return (cardSection.value = 'card-section')
  } else {
    return (cardSection.value = 'unavailable-card-section')
  }
}
</script>
