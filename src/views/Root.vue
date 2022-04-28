<template>
  <div class="root-container">
    <h1 class="root-title">結帳</h1>
    <div class="main">
      <RootCheckout
        id="root-checkout"
        @afterNext="updateData"
        @afterSubmit="printData"
      />
      <RootCart
        id="root-cart"
        :cartData="cartData"
        @afterAddBtn="addProductAmount"
        @afterMinusBtn="MinusProductAmount"
      />
    </div>
  </div>
</template>

<script>
import RootCheckout from "../components/RootCheckout.vue";
import RootCart from "../components/RootCart.vue";

const dummyCartProducts = [
  {
    productId: 0,
    productName: "破壞補丁牛仔褲",
    productImg: require('../assets/product-1.png'),
    productAmount: 1,
    productPrice: 3999,
  },
  {
    productId: 1,
    productName: "刷色直筒牛仔褲",
    productImg: require('../assets/product-2.png'),
    productAmount: 1,
    productPrice: 1299,
  },
];

export default {
  components: {
    RootCheckout,
    RootCart,
  },
  data() {
    return {
      formData: {
        gender: "",
        name: "",
        phone: "",
        email: "",
        city: "",
        address: "",
        deliveryType: "standard",
        cardOwnerName: "",
        cardNumber: "",
        cardValidity: "",
        cardCvc: "",
      },
      cartData: {
        products: dummyCartProducts,
        deliveryType: "standard",
        deliveryFee: 0,
      },
    };
  },
  watch: {
    "formData.deliveryType": function () {
      this.cartData.deliveryType = this.formData.deliveryType;
      this.cartData.deliveryFee =
        this.cartData.deliveryType === "standard" ? 0 : 500;
    },
  },
  methods: {
    printData(e) {
      this.formData = e;
      console.log("Order", this.formData, this.cartData);
    },
    updateData(e) {
      this.formData = e;
    },
    addProductAmount(productId) {
      const product = dummyCartProducts.find(function (item) {
        return item.productId === productId;
      });
      product.productAmount++;
    },
    MinusProductAmount(productId) {
      const product = dummyCartProducts.find(function (item) {
        return item.productId === productId;
      });
      if (product.productAmount === 0) {
        return;
      }
      product.productAmount--;
    },
  },
};
</script>

<style scoped>
@import "../css/resetCSS.css";

.root-container {
  width: 80%;
  margin: 0 auto;
}

.root-title {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 48px;
}

.main {
  display: flex;
  justify-content: space-between;
}

#root-checkout {
  width: 50%;
}

#root-cart {
  width: 40%;
}
</style>