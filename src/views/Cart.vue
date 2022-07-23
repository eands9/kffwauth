<template>
  <div style="margin-top: 35px; height: 2000px">
    <CartItemCard
      v-for="product in products"
      :key="product.id"
      :product="product"
    />
    <CartSummaryPaymentCard />
    <div>
        <a
          v-if="userInfo"
          :href="`/.auth/logout?post_logout_redirect_uri=https://agreeable-forest-09f155110.1.azurestaticapps.net/cart`"
          >Logout</a
        >
        <a
          v-if="!userInfo"
          :href="`/.auth/login/aad?post_login_redirect_uri=https://agreeable-forest-09f155110.1.azurestaticapps.net/cart`"
          >Login</a
        >
    <div class="user" v-if="userInfo">
      <p>Welcome</p>
      <p>{{ userInfo.userDetails }}</p>
    </div>
    </div>
  </div>
</template>

<script>
import CartItemCard from '../components/cart/CartItemCard.vue';
import CartSummaryPaymentCard from '../components/cart/CartSummaryPaymentCard.vue';
export default {
  components: {
    CartItemCard,
    CartSummaryPaymentCard,
  },
  data() {
    return {
      userInfo: {
        type: Object,
        default() {},
      },
    };
  },
  computed: {
    products() {
      return this.$store.getters.cartItems;
    },
  },
  methods: {
    login(){
      this.$router.push('/.auth/login/aad')
    },
    async getUserInfo() {
      try {
        const response = await fetch('/.auth/me');
        const payload = await response.json();
        const { clientPrincipal } = payload;
        return clientPrincipal;
      } catch (error) {
        console.error('No profile could be found');
        return undefined;
      }
    },
  },
  async created() {
    this.userInfo = await this.getUserInfo();
  },
};
</script>
