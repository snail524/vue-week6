<template>
  <h2> 單一產品列表</h2>
  <div class="container">
    <!-- {{product}} -->
    {{product.title}}
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: [],
    };
  },
  methods: {
    getProdcuts() {
      // $router -> 方法
      // $route -> 物件
      const { id } = this.$route.params;
      this.$http(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/product/${id}`)
        .then((res) => {
          console.log(res);
          this.product = res.data.product;
        })
        .catch((rej) => {
          console.dir(rej);
        });
    },
  },
  mounted() {
    this.getProdcuts();
  },
};
</script>
