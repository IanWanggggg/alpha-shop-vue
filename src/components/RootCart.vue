<template>
  <div class="root-cart-container">
    <div class="cart">
      <h1 class="cart-title">購物籃</h1>
      <div class="cart-products">
        <div
          v-for="product in cartData.products"
          :key="product.productId"
          class="cart-product"
        >
          <img
            :src="product.productImg"
            alt="不知道怎麼抓路徑"
            class="cart-product-img"
            height="100px"
            width="100px"
          />
          <div class="cart-product-amount">
            <h1 class="cart-product-amount-name">{{ product.productName }}</h1>
            <div class="cart-product-amount-number">
              <button
                @click.stop.prevent="btnMinusAmount(product.productId)"
                class="amount-number-minus amount-number-btn"
              >
                -
              </button>
              <h1 class="amount-number-number">{{ product.productAmount }}</h1>
              <button
                @click.stop.prevent="btnAddAmount(product.productId)"
                class="amount-number-plus amount-number-btn"
              >
                +
              </button>
            </div>
            <h1 class="cart-product-price">${{ product.productPrice }}</h1>
          </div>
        </div>
      </div>
      <div class="cart-deliveryFee">
        <h1 class="cart-deliveryFee-title">運費</h1>
        <h1 class="cart-deliveryFee-fee">
          {{ cartData.deliveryFee | deliveryFeeIsFree }}
        </h1>
      </div>
      <div class="cart-total">
        <h1 class="cart-total-title">小計</h1>
        <h1 class="cart-total-number">${{ total }}</h1>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cartData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      total:
        this.cartData.products[0].productPrice *
          this.cartData.products[0].productAmount +
        this.cartData.products[1].productPrice *
          this.cartData.products[1].productAmount +
        this.cartData.deliveryFee,
    };
  },
  methods: {
    btnAddAmount(productId) {
      this.$emit("afterAddBtn", productId);
    },
    btnMinusAmount(productId) {
      this.$emit("afterMinusBtn", productId);
    },
  },
  updated() {
    this.total =
      this.cartData.products[0].productPrice *
        this.cartData.products[0].productAmount +
      this.cartData.products[1].productPrice *
        this.cartData.products[1].productAmount +
      this.cartData.deliveryFee;
  },
  filters: {
    deliveryFeeIsFree(fee) {
      return fee === 0 ? "免費" : "500";
    },
  },
};
</script>

<style scoped>
.root-cart-container {
  border: 1px solid #f0f0f5;
  border-radius: 8px;
  padding: 32px 24px;
}

.cart-title {
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 32px;
}

.cart-product {
  display: flex;
  position: relative;
  margin-bottom: 32px;
}

.cart-product-amount {
  margin-left: 21px;
}

.cart-product-amount-name {
  font-size: 16px;
  font-weight: 400;
  margin-bottom: 18px;
}

.cart-product-amount-number {
  display: flex;
  align-items: center;
}

.amount-number-btn {
  height: 26px;
  width: 26px;
  border-radius: 50%;
  border: 0;
  font-size: 18px;
}

.amount-number-number {
  font-size: 14px;
  font-weight: 500;
  width: 53px;
  text-align: center;
}

.cart-product-price {
  position: absolute;
  top: 0;
  right: 0;
  font-size: 16px;
  font-weight: 700;
}

.cart-deliveryFee {
  display: flex;
  justify-content: space-between;
  padding: 16px 0;
  margin-bottom: 16px;
  border-top: 1px solid #f0f0f5;
}

.cart-deliveryFee-title {
  font-size: 14px;
  font-weight: 400;
}

.cart-deliveryFee-fee {
  font-size: 14px;
  font-weight: 700;
}

.cart-total {
  display: flex;
  justify-content: space-between;
  padding: 16px 0;
  border-top: 1px solid #f0f0f5;
}

.cart-total-title {
  font-size: 14px;
  font-weight: 400;
}

.cart-total-number {
  font-size: 14px;
  font-weight: 700;
}
</style>