<script setup>
import { onMounted, ref } from 'vue';
import ProductCard from './ProductCard.vue';
import api from '../services/api';

const props = defineProps(['id', 'name']);
const products = ref([]);

const fetchProducts = async (id) => {
  await api.get(`/categories/${id}/products?offset=0&limit=12`)
    .then((response) => {
      products.value = response.data;
    })
    .catch(error => console.error(error));
}

onMounted(fetchProducts(props.id));
</script>

<template>
  <section class="category">
    <p>{{ name }}</p>

    <div class="category__products">
      <ProductCard v-for="product in products" :key="product.id" :img="product.images[0]"
        :title="product.title" :price="product.price" :id="product.id" />
    </div>
  </section>
</template>

<style scoped>
.category {
  margin-bottom: 1.5rem;
}

.category p {
  margin-bottom: .5rem;
}

.category__products {
  display: grid;
  grid-template-columns: 1fr;
  text-align: center;
}

@media (min-width: 550px) {
  .category__products {
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
    text-align: left;
  }
}

@media (min-width: 850px) {
  .category__products {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 1200px) {
  .category__products {
    grid-template-columns: repeat(4, 1fr);
  }
}

</style>