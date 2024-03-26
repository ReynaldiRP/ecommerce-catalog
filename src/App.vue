<template>
  <body>
    <ProductSection
      :showNextSection="showNextSection"
      :dataProduct="dataProduct"
      :circleClasses="circleClasses"
      :formattedRating="formattedRating"
      :currentSection="checkCurrentSection"
    />
    <!-- <UnavailableSection :showNextSection="showNextSection" v-else /> -->
  </body>
  <!-- <RouterView /> -->
</template>

<script setup>
// import { RouterLink, RouterView } from 'vue-router'
// import UnavailableSection from '@/components/UnavailableSection.vue'
import ProductSection from '@/components/ProductSection.vue'

import { ref, computed, onMounted } from 'vue'
const dataProduct = ref({
  title: '',
  category: '',
  rating: 0,
  image: '',
  description: '',
  price: 0
})

const currentIdProduct = ref(1)
let className = ref({})

const fetchData = async () => {
  try {
    const url = `https://fakestoreapi.com/products/${currentIdProduct.value++}`
    let response = await fetch(url)
    let data = await response.json()
    return data
  } catch (error) {
    console.error('Error fetching data:', error)
    return null
  }
}

onMounted(async () => {
  await fetchDataAndUpdate()
})

const fetchDataAndUpdate = async () => {
  const data = await fetchData()
  if (data) {
    if (currentIdProduct.value >= 20) {
      currentIdProduct.value = 1
    }
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

const showNextSection = async () => {
  await fetchDataAndUpdate()
}
</script>
