<template>
  <body>
    <section :class="checkProductSection()" id="background-section">
      <CardProduct> </CardProduct>
    </section>
  </body>

  <!-- <RouterView /> -->
</template>

<script setup>
// import { RouterLink, RouterView } from 'vue-router'
import CardProduct from './components/CardProduct.vue'

import { ref } from 'vue'

const productSection = ref(['women-section'])

const checkProductSection = () => {
  const newValue = productSection.value

  if (newValue.length > 0) {
    if (newValue === 'men-section') {
      productSection.value = 'men-section'
    } else if (newValue === 'women-section') {
      productSection.value = 'women-section'
    }
    return productSection.value
  } else {
    return (productSection.value = 'unavailable-section')
  }
}
</script>
