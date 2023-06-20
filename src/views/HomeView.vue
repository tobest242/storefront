<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
        <div class="hero-body has-text-centered">
            <p class="title mb-6">
                Welcome to MyStore
            </p>
            <p class="subtitle">
                The best store online
            </p>
        </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
          <h2 class="is-size-2 has-text-centered">Our Products</h2>
          
      </div>

      <ProductBox 
        v-for="product in Products"
        v-bind:key="product.id"
        v-bind:product="product" />
      
  </div>
  </div>
</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      Products: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getProducts()

    document.title = 'Home | MyStore'
  },
  methods: {
    getProducts() {
      axios
      .get('/api/products/')
      .then(response => {
        this.Products = response.data
      })
      .catch(error => {
        console.log(error)
      })
    }
  }
}
</script>