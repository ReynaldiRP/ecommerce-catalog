<script setup>
import CardProduct from '@/components/CardProduct.vue'
const UnavailableSection = defineAsyncComponent(() => import('@/components/UnavailableSection.vue'))
const ProductSectionSkeleton = defineAsyncComponent(
  () => import('@/components/ProductSectionSkeleton.vue')
)

import { ref, computed, onMounted, defineAsyncComponent } from 'vue'

const dataProduct = ref({
  title: '',
  category: '',
  rating: '',
  image: '',
  description: '',
  price: 0
})

const currentIdProduct = ref(20)
const isLoading = ref(false)
let className = ref({})

const fetchData = async () => {
  try {
    const url = `https://fakestoreapi.com/products/${currentIdProduct.value}`
    let response = await fetch(url)
    let data = await response.json()
    return data
  } catch (error) {
    console.error('There is no Data:', error)
    return null
  }
}

onMounted(async () => {
  await fetchDataAndUpdate()
})

const fetchDataAndUpdate = async () => {
  const data = await fetchData()
  if (data) {
    dataProduct.value = {
      title: data.title,
      category: data.category,
      rating: data.rating.rate,
      image: data.image,
      description: data.description,
      price: data.price
    }
  }
}

const showNextSection = async () => {
  isLoading.value = true
  if (currentIdProduct.value >= 20) {
    currentIdProduct.value = 0
  } else {
    currentIdProduct.value += 1
  }
  await fetchDataAndUpdate()
  isLoading.value = false
}

await fetchDataAndUpdate((resolve) => {
  setTimeout(() => {
    resolve()
  }, 3000)
})

const circleClasses = computed(() => {
  const classes = []
  const rating = Math.round(dataProduct.value.rating)
  for (let i = 0; i < 5; i++) {
    classes.push({ circle: true, active: i < rating })
  }
  return classes
})

const formattedRating = computed(() => {
  return `${dataProduct.value.rating}/5`
})

const checkCurrentSection = () => {
  const category = dataProduct.value.category
  if (category === `men's clothing`) {
    return (className.value = {
      section: 'men-section',
      title: 'men-clothes-title',
      rating: 'circle-men',
      price: 'men-clothes-price',
      buttonBuy: 'men-button-buy',
      buttonNext: 'men-button-next'
    })
  } else if (category === `women's clothing`) {
    return (className.value = {
      section: 'women-section',
      title: 'women-clothes-title',
      rating: 'circle-women',
      price: 'women-clothes-price',
      buttonBuy: 'women-button-buy',
      buttonNext: 'women-button-next'
    })
  }
  return (className.value = {
    section: 'unavailable-section',
    title: 'unavailable-primary-text',
    rating: 'circle-unavailable',
    price: 'unvailable-clothes-price',
    buttonBuy: 'unavailable-button-buy',
    buttonNext: 'unavailable-button-next'
  })
}
</script>

<template>
  <UnavailableSection v-if="currentIdProduct === 0" :showNextSection="showNextSection" />
  <section v-else :class="checkCurrentSection().section" id="background-section">
    <CardProduct class="card-section">
      <img :src="dataProduct.image" id="clothesImage" />
      <article class="clothes-description">
        <h1 :class="checkCurrentSection().title" id="clothes-title">
          {{ dataProduct.title }}
        </h1>
        <section class="clothes-categories">
          <p class="clothes-category">{{ dataProduct.category }}</p>
          <section class="clothes-rating">
            <p>{{ formattedRating }}</p>
            <div class="rating-indicator">
              <div
                v-for="(circle, index) in circleClasses"
                :key="index"
                :class="[circle, checkCurrentSection().rating]"
              ></div>
            </div>
          </section>
        </section>
        <div class="border-bottom"></div>
        <p class="clothes-description">
          {{ dataProduct.description }}
        </p>
        <section class="clothes-footer">
          <div class="border-bottom"></div>
          <h1 :class="checkCurrentSection().price" id="clothes-price">${{ dataProduct.price }}</h1>
          <div class="button-group">
            <button :class="checkCurrentSection().buttonBuy">Buy now</button>
            <button @click="showNextSection()" :class="checkCurrentSection().buttonNext">
              Next Product
            </button>
          </div>
        </section>
      </article>
    </CardProduct>
  </section>
  <ProductSectionSkeleton v-if="isLoading" />
</template>
