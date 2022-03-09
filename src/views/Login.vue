<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-6">
      <h1 class="h3 mb-3 font-weight-normal">
        請先登入
      </h1>
      <div class="col-8">
        <form id="form" class="form-signin">
          <div class="form-floating mb-3">
            <input type="email" class="form-control" id="username"
              placeholder="name@example.com" v-model='user.username' required autofocus>
            <label for="username">Email address</label>
          </div>
          <div class="form-floating">
            <input type="password" class="form-control" id="password"
              placeholder="Password" v-model='user.password' required>
            <label for="password">Password</label>
          </div>
          <button class="btn btn-lg btn-primary w-100 mt-3" type="submit"
          @click.prevent="signIn()">
            登入
          </button>
        </form>
      </div>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-6">
        <p class="mt-5 mb-3 text-muted">
          &copy; 2021~∞ - 六角學院
        </p>
    </div>
    </div>
  </div>

</template>

<script>
export default {
  data() {
    return {
      user: {},
    };
  },
  methods: {
    signIn() {
      this.$http.post(`${process.env.VUE_APP_API}/admin/signin`, this.user)
        .then((res) => {
          console.log('login成功', res);
          const { token, expired } = res.data;
          document.cookie = `hexToken=${token}; expires=${new Date(expired)}; `;
          this.$router.push('/admin/products');
        })
        .catch((rej) => {
          console.dir(rej.response);
          alert(rej.response.data.message);
        });
    },
  },
};
</script>
