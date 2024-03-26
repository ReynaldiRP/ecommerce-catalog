<template>
  <section :class="currentSection().section" id="background-section">
    <CardProduct class="card-section">
      <img :src="dataProduct.image" class="skeleton-img" id="clothesImage" />
      <article class="clothes-description">
        <h1 :class="currentSection().title" id="clothes-title">
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
                :class="[circle, currentSection().rating]"
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
          <h1 :class="currentSection().price" id="clothes-price">${{ dataProduct.price }}</h1>
          <div class="button-group">
            <button :class="currentSection().buttonBuy">Buy now</button>
            <button @click="showNextSection" :class="currentSection().buttonNext">
              Next Product
            </button>
          </div>
        </section>
      </article>
    </CardProduct>
  </section>
</template>

<script setup>
import CardProduct from '@/components/CardProduct.vue'

defineProps({
  showNextSection: Function,
  dataProduct: Object,
  circleClasses: Array,
  formattedRating: String,
  currentSection: Function
})
</script>
