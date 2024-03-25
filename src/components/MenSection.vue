<template>
  <section class="men-section" id="background-section">
    <CardProduct class="card-section">
      <img src="../assets/" alt="men-clothes" />
      <article class="clothes-description">
        <h1 class="men-clothes-title" id="clothes-title">{{ menClothes.title }}</h1>
        <section class="clothes-categories">
          <p class="clothes-category">{{ menClothes.category }}</p>
          <section class="clothes-rating">
            <p>{{ menClothes.rating }}</p>
            <div>rating</div>
          </section>
        </section>
        <div class="border-bottom"></div>
        <p class="clothes-description">
          {{ menClothes.description }}
        </p>
        <section class="clothes-footer">
          <div class="border-bottom"></div>
          <h1 class="men-clothes-price" id="clothes-price">${{ menClothes.price }}</h1>
          <div class="button-group">
            <button class="men-button-buy">Buy now</button>
            <button @click="showNextSection" class="men-button-next">Next Product</button>
          </div>
        </section>
      </article>
    </CardProduct>
  </section>
</template>

<script setup>
import CardProduct from '@/components/CardProduct.vue'
import { ref } from 'vue'

const menClothes = ref({
  title: 'Mens Casual Slim Fit',
  category: 'mens clothing',
  rating: '2.9/5',
  description: `great outerwear jackets for Spring/Autumn/Winter, suitable for many occasions, such as
          working, hiking, camping, mountain/rock climbing, cycling, traveling or other outdoors.
          Good gift choice for you or your family member. A warm hearted love to Father, husband or
          son in this thanksgiving or Christmas Day.`,
  price: 29.95
})

defineProps({
  showNextSection: Function
})
</script>
