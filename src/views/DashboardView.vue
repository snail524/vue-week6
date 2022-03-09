<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container-fluid">
     <router-link class="navbar-brand" to="/admin">後台</router-link>
    <button class="navbar-toggler" type="button"
    data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
    aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link" to="/">前台首頁</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin">後台首頁</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin/products">產品列表</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin/coupon">優惠卷</router-link>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click.prevent="logout">登出</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
  <router-view v-if="checkSuccess"></router-view>
</template>

<script>
export default {
  data() {
    return {
      checkSuccess: false,
    };
  },
  mounted() {
    this.checkLogin();
  },
  methods: {
    checkLogin() {
      const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
      this.$http.defaults.headers.common.Authorization = `${token}`;
      const api = `${process.env.VUE_APP_API}/api/user/check`;
      this.$http
        .post(api, { api_token: `${token}` })
        .then(() => {
          this.checkSuccess = true; // 通過 check api 驗證後改為 true
        })
        .catch((err) => {
          console.log(err.data);
          this.$router.push('/login');
        });
    },
    logout() {
      document.cookie = 'hexToken=;expires=;';
      this.$router.push('/login');
      alert('成功登出');
    },
  },
};
</script>
