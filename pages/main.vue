<template>
  <div>
    <Logo></Logo>
    <!--    <h1>메인 페이지</h1>-->
    <p>메인 페이지입니다.</p>
    <div>
      {{ products }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import NuxtLogo from '~/components/NuxtLogo.vue'

export default {
  async asycnData() {
    const response = await axios.get('http:///localhost:3000/products');
    console.log(response);
    this.products = response.data;
  },
  data() {
    return {
      products: []
    }
  },
  components: {
    Logo: NuxtLogo
  }
}
</script>

<style></style>
