<template>
    <div v-if="product.category == 'men\'s clothing' || product.catagory == 'women\'s clothing'">
        <div v-bind:class="product.category == 'women\'s clothing' ? 'women' : 'men'">
            <div class="container">
                <div class="images">
                    <!-- img from api -->
                    <img :src="product.image" alt="product image">
                </div>
                <div class="product">
                    <h1>{{ product.title }}</h1>
                    <div class="justify">
                        <h6>{{ product.category }}</h6>
                        <h6>
                            {{ product.rating['rate'] }} /5
                            <span v-for="n in 5" :key="n" class="dot" :class="{active: n <= product.rating['rate']}"></span>
                        </h6>
                    </div>
                    <hr />
                    <p class="desc">
                        {{ product.description }}
                    </p>
                    <hr />
                    <h2>${{ product.price }}</h2>
                    <button v-bind:class="product.category == 'men\'s clothing' ? 'button-men' : 'button'">Buy Now</button>
                    <button-next v-bind:class="product.category == 'men\'s clothing' ? 'button-next-men' : 'button'" @click="nextProduct">Next Product</button-next>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <div class="nodata">
            <div class="container">
                <div class="hero-image">
                    <div class="product">
                        <div class="fof">
                            <h1>THIS IS UNAVAILABLE TO SHOW</h1>
                        </div>
                        <button-nodata @click="nextProduct">Next Product</button-nodata>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'

export default {
  name: 'ProductDisplay',
  setup() {
    const product = ref({})
    const clothingProducts = ref([])
    let index = 1

    const fetchProduct = async () => {
      try {
        const response = await axios.get(`https://fakestoreapi.com/products/${index}`)
        const productData = response.data
        if (productData.category === "men\'s clothing" || productData.category === "women\'s clothing") {
          clothingProducts.value.push(productData)
        }
        product.value = productData
      } catch (error) {
        console.log(error)
      }
    }

    onMounted(() => {
      fetchProduct()
    })

    const nextProduct = () => {
      index++
      if (index > 20) {
        index = 1
      }
      fetchProduct()
    }

    return { product, clothingProducts, nextProduct }
  }
}
</script>

<style lang="css" scoped>
    .men{
        width: 100%;
        display: grid;
        background-image: linear-gradient(#d6e6ff 70%,white 30%);
    }

    .women{
        width: 100%;
        display: grid;
        background-image: linear-gradient(#fde2ff 70%, white 30%);
    }

    .nodata{
        width: 100%;
        display: grid;
        background-image: linear-gradient(#dcdcdc 70%, white 30%);
        font-family: sans-serif;
        text-transform: uppercase;
    }

    .dot{
        height: 15px;
        width: 15px;
        background-color: #bbb;
        border-radius:50%;
        display: inline-block;
        margin-right: 5px;
    }

    .active{
        background-color: #002772;
    }

    @import '../assets/main.css'
</style>