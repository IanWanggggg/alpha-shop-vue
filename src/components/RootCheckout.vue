<template>
  <div class="root-checkout-container">
    <div class="stepper-container">
      <div
        class="step"
        :class="[
          { 'step-active': nowStep === 1 },
          { 'step-checked': nowStep > 1 },
        ]"
      >
        <span class="step-circle"></span>
        <h1 class="step-title">寄送地址</h1>
      </div>

      <span class="connect-line"></span>

      <div
        class="step"
        :class="[
          { 'step-active': nowStep === 2 },
          { 'step-checked': nowStep > 2 },
        ]"
      >
        <span class="step-circle"></span>
        <h1 class="step-title">運送方式</h1>
      </div>

      <span class="connect-line"></span>

      <div class="step" :class="[{ 'step-active': nowStep === 3 }]">
        <span class="step-circle"></span>
        <h1 class="step-title">付款資訊</h1>
      </div>
    </div>

    <div class="forms-container">
      <div class="form-1" v-show="nowStep === 1">
        <h1 class="form-title">寄送地址</h1>
        <div class="form-1-input">
          <label for="gender-input" id="gender-label">稱謂</label>
          <label for="name-input" id="name-label">姓名</label>
          <select
            name="gender-input"
            id="gender-input"
            required
            v-model="formData.gender"
          >
            <option value="" disabled selected>稱謂</option>
          </select>
          <input
            type="text"
            id="name-input"
            placeholder="請輸入姓名"
            v-model="formData.name"
          />

          <label for="phone-input" id="phone-label">電話</label>
          <label for="email-input" id="email-label">Email</label>
          <input
            type="text"
            id="phone-input"
            placeholder="請輸入電話"
            v-model="formData.phone"
          />
          <input
            type="text"
            id="email-input"
            placeholder="請輸入電子郵件"
            v-model="formData.email"
          />

          <label for="city-input" id="city-label">縣市</label>
          <label for="address-input" id="address-label">地址</label>
          <select
            name="city-input"
            id="city-input"
            required
            v-model="formData.city"
          >
            <option value="" disabled selected>請選擇縣市</option>
          </select>
          <input
            type="text"
            id="address-input"
            placeholder="請輸入地址"
            v-model="formData.address"
          />
        </div>
      </div>
      <div class="form-2" v-show="nowStep === 2">
        <h1 class="form-title">運送方式</h1>
        <div class="form-2-input">
          <div class="delivery-type" :class="{'delivery-type-checked': formData.deliveryType==='standard'}">
            <label for="delivery-type-radio-standard">
              <input
                type="radio"
                name="delivery-type-radio"
                id="delivery-type-radio-standard"
                class="delivery-type-radio"
                checked
                v-model="formData.deliveryType"
                value="standard"
              />
              <div class="delivery-type-text">
                <h1 class="delivery-type-title">標準運送</h1>
                <h1 class="delivery-type-description">約 3~7 個工作天</h1>
              </div>
              <h1 class="delivery-type-fee">免費</h1>
            </label>
          </div>
          <div class="delivery-type" :class="{'delivery-type-checked': formData.deliveryType==='DHL'}">
            <label for="delivery-type-radio-DHL">
              <input
                type="radio"
                name="delivery-type-radio"
                id="delivery-type-radio-DHL"
                class="delivery-type-radio"
                v-model="formData.deliveryType"
                value="DHL"
              />
              <div class="delivery-type-text">
                <h1 class="delivery-type-title">DHL貨運</h1>
                <h1 class="delivery-type-description">48 小時內送達</h1>
              </div>
              <h1 class="delivery-type-fee">$500</h1>
            </label>
          </div>
        </div>
      </div>
      <div class="form-3" v-show="nowStep === 3">
        <h1 class="form-title">付款資訊</h1>
        <div class="form-3-input">
          <label for="card-owner-name-input" id="card-owner-name-label"
            >持卡人姓名</label
          >
          <input
            type="text"
            id="card-owner-name-input"
            placeholder="John Doe"
            v-model="formData.cardOwnerName"
          />

          <label for="card-number-input" id="card-number-label">卡號</label>
          <input
            type="text"
            id="card-number-input"
            placeholder="1111 2222 3333 4444"
            v-model="formData.cardNumber"
          />

          <label for="card-validity-input" id="card-validity-label"
            >有效期限</label
          >
          <label for="card-cvc-input" id="card-cvc-label">CVC / CCV</label>
          <input type="text" id="card-validity-input" placeholder="MM/YY" v-model="formData.cardValidity" />
          <input type="text" id="card-cvc-input" placeholder="123" v-model="formData.cardCvc" />
        </div>
      </div>
    </div>
    <div class="control-btn">
      <button class="pre-btn" @click.stop.prevent="preStep" v-if="nowStep > 1">
        上一步
      </button>
      <button
        class="next-btn"
        @click.stop.prevent="nextStep"
        v-if="nowStep < 3"
      >
        下一步
      </button>
      <button class="confirm-btn" v-if="nowStep === 3" @click.stop.prevent="formSubmit">確認下單</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nowStep: 1,
      formData: {
        gender: '',
        name: '',
        phone: '',
        email: '',
        city: '',
        address: '',
        deliveryType: 'standard',
        cardOwnerName: '',
        cardNumber: '',
        cardValidity: '',
        cardCvc: ''
      },
    };
  },
  methods: {
    nextStep() {
      this.nowStep++;
      this.$emit('afterNext',this.formData)
    },
    preStep() {
      this.nowStep--;
    },
    formSubmit() {
      this.$emit('afterSubmit',this.formData)
    }
  },
};
</script>

<style scoped>
.root-checkout-container {
  height: 492px;
  position: relative;
}

.stepper-container {
  width: 100%;
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: center;
}

.stepper-container :nth-child(1) .step-circle::before {
  content: "1";
}
.stepper-container :nth-child(3) .step-circle::before {
  content: "2";
}
.stepper-container :nth-child(5) .step-circle::before {
  content: "3";
}

.step {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
}

.step-circle {
  border: 1px solid #afb1bd;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  text-align: center;
  line-height: 24px;
  margin-right: 8px;
  color: #afb1bd;
}

.step-active .step-circle {
  border-color: black;
  color: black;
}
.step-active .step-title {
  color: black;
}

.step-checked .step-circle {
  border-color: black;
  background-color: black;
  color: white;
}
.stepper-container .step-checked .step-circle::before {
  content: "\2713";
}
.step-checked .step-title {
  color: black;
}

.step-title {
  font-size: 16px;
  font-weight: 400;
  color: #afb1bd;
}

.connect-line {
  background-color: black;
  width: 10%;
  height: 2px;
}

/*****************************form-1*******************************/

.form-1,
.form-2,
.form-3 {
  margin-top: 64px;
  height: 288px;
}

.form-title {
  font-size: 24px;
  font-weight: 700;
  margin-bottom: 24px;
}

.form-1-input {
  width: 100%;
  display: grid;
  grid-template-areas:
    "gender-label gender-label name-label name-label name-label name-label"
    "gender-input gender-input name-input name-input name-input name-input"
    "phone-label phone-label phone-label email-label email-label email-label"
    "phone-input phone-input phone-input email-input email-input email-input"
    "city-label city-label address-label address-label address-label address-label"
    "city-input city-input address-input address-input address-input address-input";
  gap: 0px 30px;
}

#gender-label {
  grid-area: gender-label;
}
#name-label {
  grid-area: name-label;
}
#gender-input {
  grid-area: gender-input;
}
#name-input {
  grid-area: name-input;
}
#phone-label {
  grid-area: phone-label;
}
#email-label {
  grid-area: email-label;
}
#phone-input {
  grid-area: phone-input;
}
#email-input {
  grid-area: email-input;
}
#city-label {
  grid-area: city-label;
}
#address-label {
  grid-area: address-label;
}
#city-input {
  grid-area: city-input;
}
#address-input {
  grid-area: address-input;
}

.form-1-input label {
  font-size: 12px;
  font-weight: 700;
  color: #808080;
  margin-bottom: 8px;
}
.form-1-input input,
select {
  border: 1px solid #5e5e5e;
  border-radius: 4px;
  height: 40px;
  margin-bottom: 24px;
  padding: 12px 16px;
}
.form-1-input select {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  background: url("../assets/arrow.png") 95% 50% no-repeat scroll transparent;
}
.form-1-input select:invalid {
  color: #808080;
}

/***************************  form-2 ***************************/

.delivery-type {
  border: 1px solid #f0f0f5;
  border-radius: 4px;
  width: 80%;
  height: 60px;
  margin-bottom: 24px;
  position: relative;
}

.delivery-type-checked {
  border-color: black;
}

.delivery-type > label {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
}

.delivery-type-radio {
  -webkit-appearance: none;
  margin: 20px;
  height: 20px;
  width: 20px;
  border: 2px solid black;
  border-radius: 50%;
}
.delivery-type-radio:checked {
  box-shadow: inset 0 0 0 4px black;
  border: 1px solid black;
}

.delivery-type-title {
  font-size: 14px;
  font-weight: 700;
  margin-bottom: 5px;
}

.delivery-type-description {
  font-size: 12px;
  font-weight: 400;
}

.delivery-type-fee {
  font-size: 12px;
  font-weight: 400;
  position: absolute;
  top: 13px;
  right: 20px;
}

/**************  form-3 **************************/

.form-3-input {
  display: grid;
  grid-template-areas:
    "card-owner-name-label card-owner-name-label card-owner-name-label card-owner-name-label . ."
    "card-owner-name-input card-owner-name-input card-owner-name-input card-owner-name-input . ."
    "card-number-label card-number-label card-number-label card-number-label . ."
    "card-number-input card-number-input card-number-input card-number-input . ."
    "card-validity-label card-validity-label card-validity-label card-cvc-label card-cvc-label card-cvc-label"
    "card-validity-input card-validity-input card-validity-input card-cvc-input card-cvc-input card-cvc-input";
  gap: 0px 30px;
}

#card-owner-name-label {
  grid-area: card-owner-name-label;
}
#card-owner-name-input {
  grid-area: card-owner-name-input;
}
#card-number-label {
  grid-area: card-number-label;
}
#card-number-input {
  grid-area: card-number-input;
}
#card-validity-label {
  grid-area: card-validity-label;
}
#card-validity-input {
  grid-area: card-validity-input;
}
#card-cvc-label {
  grid-area: card-cvc-label;
}
#card-cvc-input {
  grid-area: card-cvc-input;
}

.form-3-input label {
  font-size: 12px;
  font-weight: 700;
  color: #808080;
  margin-bottom: 8px;
}
.form-3-input input {
  border: 1px solid #5e5e5e;
  border-radius: 4px;
  height: 40px;
  margin-bottom: 24px;
  padding: 12px 16px;
}

/*******************  control-btn ******************/

.control-btn {
  border-top: 2px solid #e6e6eb;
  position: relative;
  margin-top: 24px;
  padding-top: 24px;
  height: 72px;
}

.pre-btn {
  position: absolute;
  left: 0;
  width: 174px;
  height: 46px;
  border: 0;
  border-radius: 8px;
  background-color: white;
  cursor: pointer;
}

.next-btn,
.confirm-btn {
  position: absolute;
  right: 0;
  width: 174px;
  height: 46px;
  border: 0;
  border-radius: 8px;
  background-color: #f67599;
  cursor: pointer;
  color: white;
}
</style>