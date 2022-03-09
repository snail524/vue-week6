<template>
  <h2> 產品列表</h2>
  <div class="container">
    <div class="row row-cols-1 row-cols-md-3 g-2">
      <div class="col" v-for="item in products" :key="item.id">
        <div class="card h-100" >
          <img :src="item.imageUrl" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ item.title }}</h5>
            <p class="card-text"> {{ item.description }}</p>
            <router-link :to="`/product/${item.id}`" class="btn btn-primary">
            詳情</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
    };
  },
  methods: {
    getProdcuts() {
      this.$http(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`)
        .then((res) => {
          console.log(res.data.products);
          this.products = res.data.products;
        });
    },
  },
  mounted() {
    this.getProdcuts();
  },
};
</script>
