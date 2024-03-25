<template>
  <section class="women-section" id="background-section">
    <CardProduct class="card-section">
      <img :src="womenClothes.image" alt="women-clothes" id="clothesImage" />
      <article class="clothes-description">
        <h1 class="women-clothes-title" id="clothes-title">{{ womenClothes.title }}</h1>
        <section class="clothes-categories">
          <p class="clothes-category">{{ womenClothes.category }}</p>
          <section class="clothes-rating">
            <p>{{ formattedRating }}</p>
            <div class="rating-indicator">
              <div v-for="(circle, index) in circleClasses" :key="index" :class="circle"></div>
            </div>
          </section>
        </section>
        <div class="border-bottom"></div>
        <p class="clothes-description">
          {{ womenClothes.description }}
        </p>
        <section class="clothes-footer">
          <div class="border-bottom"></div>
          <h1 class="women-clothes-price" id="clothes-price">${{ womenClothes.price }}</h1>
          <div class="button-group">
            <button class="women-button-buy">Buy now</button>
            <button @click="showNextSection" class="women-button-next">Next Product</button>
          </div>
        </section>
      </article>
    </CardProduct>
  </section>
</template>

<script setup>
import CardProduct from '@/components/CardProduct.vue'
import { ref, computed, onMounted } from 'vue'

const fetchData = async () => {
  try {
    let response = await fetch('https://fakestoreapi.com/products/2')
    let data = await response.json()
    return data
  } catch (error) {
    console.error('Error fetching data:', error)
    return null
  }
}

const womenClothes = ref({
  title: '',
  category: '',
  rating: 0,
  image: '',
  description: '',
  price: 0
})

onMounted(async () => {
  const data = await fetchData()
  if (data) {
    womenClothes.value = {
      title: data.title,
      category: data.category,
      rating: data.rating.rate,
      image: data.image,
      description: data.description,
      price: data.price
    }
  }
})

const formattedRating = computed(() => {
  return `${womenClothes.value.rating}/5`
})

defineProps({
  showNextSection: Function,
  circleClasses: Function
})
</script>
