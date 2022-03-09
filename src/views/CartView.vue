<template>
  <h2> 購物車</h2>
   <table class="table align-middle">
            <thead>
              <tr>
                <th>圖片</th>
                <th>商品名稱</th>
                <th>價格</th>
                <th></th>
              </tr>
            </thead>
            <tbody v-for="item in cartProducts" :key="item.id">
              <tr>
                <td style="width: 200px">
                  <div :style="{backgroundImage : `url(${item.imageUrl})`}"
                  style="height: 100px;  background-size: cover; background-position: center"></div>
                </td>
                <td>
                  {{ item.title }}
                </td>
                <td>
                  <div class="h5" v-if="item.origin_price=== item.price">{{ item.price }} 元</div>
                  <div v-else>
                    <del class="h6">原價 {{ item.origin_price }} 元</del>
                     <div class="h5">現在只要 {{ item.price }} 元</div>
                  </div>
                </td>
                <td>
                  <div class="btn-group btn-group-sm">
                    <button type="button" class="btn btn-outline-secondary"
                    @click="openProdcutModal(item.id)">
                      <span class="spinner-border spinner-border-sm"
                         role="status" aria-hidden="true" v-if="isloadingItem===item.id"></span>
                      查看更多
                    </button>
                    <button type="button" class="btn btn-outline-danger"
                    @click="addToCart(item.id)">
                        <span class="spinner-border spinner-border-sm"
                         role="status" aria-hidden="true" v-if="isloadingItem===item.id"></span>
                     加到購物車
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <div class="text-end">
            <button class="btn btn-outline-danger" @click="removeCartAll"
             type="button">清空購物車</button>
          </div>
          <table class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>品名</th>
                <th style="width: 150px">數量/單位</th>
                <th>單價</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="cartsData">
                <tr v-for="item in cartsData.carts" :key="item.id">
                  <td>
                    <button type="button" class="btn btn-outline-danger btn-sm"
                    @click="removeCartItem(item.id)">
                      <i class="fas fa-spinner fa-pulse" v-if="isloadingItem=== item.id"></i>
                      {{ cartsData.carts.id}}
                      <!-- <i class="fas fa-spinner fa-pulse" v-else></i> -->
                      x
                    </button>
                  </td>
                  <td>
                    {{ item.product.title }}
                    <div class="text-success">
                      已套用優惠券
                    </div>
                  </td>
                  <td>
                    <div class="input-group input-group-sm">
                      <div class="input-group mb-3">
                        <!-- <input min="1"
                             type="number" class="form-control" v-model.number="item.qty"/> -->
                        <select name="" id="" class="form-select" v-model="item.qty"
                        @change="upDateToCart(item)" :disabled="isloadingItem=== item.id">
                          <option :value="num" v-for="num in 20"
                          :key="`${num}${item.id}`" :selected="item.qty===num">{{num}}</option>
                        </select>
                        <span class="input-group-text" id="basic-addon2">
                          {{ item.product.unit }}</span>
                      </div>
                    </div>
                  </td>
                  <td class="text-end">
                    <!-- <small class="text-success">折扣價：</small> -->
                    {{ item.total }}
                  </td>
                </tr>
              </template>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" class="text-end">總計</td>
                <td class="text-end">  {{ cartsData.total }}</td>
              </tr>
              <tr>
                <td colspan="3" class="text-end text-success">折扣價</td>
                <td class="text-end text-success">{{  }}</td>
              </tr>
            </tfoot>
          </table>
</template>

<script>
import emitter from '@/libs/emitter';

export default {
  data() {
    return {
      products: [],
      cartProducts: {},
      cartsData: {},
      isloadingItem: '',
    };
  },
  components: {

  },
  methods: {
    getProdcuts() {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`)
        .then((res) => {
          this.cartProducts = res.data.products;
        })
        .catch((rej) => {
          console.log(rej);
        });
    },
    openProdcutModal(id) {
      this.productId = id;
      this.$refs.productModal.openModal();
      // 用Refs吃到元件內的dom，並利用其中的openmodal
    },
    addToCart(id, qty = 1) {
      const data = {
        product_id: id,
        qty,
      };
      this.isloadingItem = id;
      this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data })
        .then((res) => {
          console.log(res.data.data);
          this.getCart();
          this.isloadingItem = '';
          emitter.emit('get-cart');
          // this.$refs.productModal.closeModal();
        })
        .catch((rej) => {
          console.log(rej);
        });
    },
    getCart() {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          console.log('cart', res.data.data);
          this.cartsData = res.data.data;
        })
        .catch((rej) => {
          console.log(rej);
        });
    },
    removeCartItem(id) {
      console.log(id);
      this.isloadingItem = id;
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${id}`)
        .then((res) => {
          console.log(res);
          this.getCart();
          this.isloadingItem = '';
        });
    },
    removeCartAll() {
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`)
        .then((res) => {
          console.log(res);
          this.getCart();
        });
    },
  },
  mounted() {
    this.getProdcuts();
    this.getCart();
  },
};
</script>
