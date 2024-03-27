<script setup>
// import { RouterLink, RouterView } from 'vue-router'
import { defineAsyncComponent } from 'vue'

const ProductSection = defineAsyncComponent(() => import('@/components/ProductSection.vue'))
const ProductSectionSkeleton = defineAsyncComponent(
  () => import('@/components/ProductSectionSkeleton.vue')
)
</script>

<template>
  <body>
    <Suspense :timeout="0">
      <template #default>
        <ProductSection />
      </template>
      <template #fallback>
        <ProductSectionSkeleton />
      </template>
    </Suspense>
  </body>
  <!-- <RouterView /> -->
</template>
