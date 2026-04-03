<template>
  <div class="app">
    <!--    <Logo></Logo>-->
    <main>
      <SearchInput
        :search-keyword="searchKeyword"
        @input="updateSearchKeyword"
        v-model="searchKeyword"
        @search="searchProducts"
      ></SearchInput>
      <ul>
        <li
          class="item flex"
          v-for="product in products"
          :key="product.id"
          @click="moveToDetailPage(product.id)"
        >
          <img
            class="product-image"
            :src="product.imageUrl"
            :alt="product.name"
          />
          <p>{{ product.name }}</p>
          <span>{{ product.price }}</span>
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
// import NuxtLogo from '~/components/NuxtLogo.vue'
import SearchInput from '@/components/SearchInput.vue'
import { fetchProductsByKeyword } from '~/api'

export default {
  async asyncData() {
    // asyncData는 페이지 컴포넌트(pages 폴더 아래에 위치하는 컴포넌트)에만 제공되는 속성
    const response = await axios.get('http:///localhost:3000/products')
    const products = response.data.map((item) => ({
      ...item,
      imageUrl: `${item.imageUrl}?random=${Math.random()}`,
    }))
    return { products } // 로컬 data에 추가
  },
  methods: {
    moveToDetailPage(id) {
      this.$router.push(`detail/${id}`)
    },
    updateSearchKeyword(keyword) {
      this.searchKeyword = keyword
    },
    async searchProducts() {
      const response = await fetchProductsByKeyword(this.searchKeyword)
      console.log(response.data)
      this.products = response.data.map((item) => ({
        ...item,
        imageUrl: `${item.imageUrl}?random=${Math.random()}`,
      }))
    },
  },
  data() {
    return {
      searchKeyword: '',
    }
  },
  components: {
    // Logo: NuxtLogo,
    SearchInput,
  },
}
</script>

<style scoped>
.flex {
  display: flex;
  justify-content: center;
}
.item {
  display: inline-block;
  width: 400px;
  height: 300px;
  text-align: center;
  margin: 0 0.5rem;
  cursor: pointer;
}
.product-image {
  width: 400px;
  height: 250px;
}
.app {
  position: relative;
}
.cart-wrapper {
  position: sticky;
  float: right;
  bottom: 50px;
  right: 50px;
}
.cart-wrapper .btn {
  display: inline-block;
  height: 40px;
  font-size: 1rem;
  font-weight: 500;
}
</style>
