<template>
  <FrontNavbar></FrontNavbar>
  <router-view></router-view>
  <!-- productsview -->
</template>

<script>
import FrontNavbar from '@/components/FrontNavbar.vue';

export default {
  components: {
    FrontNavbar,
  },
};
</script>
