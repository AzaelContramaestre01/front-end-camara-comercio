<template>
  <div>
    <VmHero />
    <VmSubheader />
    <VmProductsList />
  </div>
</template>

<script>
import VmProductsList from '@/components/products_list/ProductsListContainer';
import VmHero from '@/components/hero/Hero';
import VmSubheader from '@/components/subheader/Subheader'

export default {
  name: 'index',
  components: {
    VmProductsList,
    VmHero,
    VmSubheader
  },

  data () {
    return {
      products: [],
      finalProducts: [],
      strapiUrl: process.env.strapiUrl
    }
  },

  mounted() {
    const url = `${this.strapiUrl}/api/products`
    const imageURL = `${this.strapiUrl}/api/products?populate=*`
    this.$axios.get(url)
      .then(response => {
        this.products = response.data.data
        this.$axios.get(imageURL)
          .then(response => {
            this.finalProducts = response.data.data.map(product => {
              return {
                ...product.attributes,
                id: product.id,
                img: `http://localhost:1337${response.data.data.filter(item => item.id === product.id)[0].attributes.img.data.attributes.url}`
              }
            })
            this.$store.commit('setProducts', this.finalProducts)
          })
          .catch(error => {
            console.log("Fetch to products images failed", error)
          })
      })
      .catch(error => {
        console.log("Fetch to products failed", error)
      })
  }
}
</script>
