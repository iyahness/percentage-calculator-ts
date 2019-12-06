<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <div class="statement-container">
          <h2 class="statement text-uppercase">
            I want to know
          </h2>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <div class="choices">
          <b-form-select v-model="selected" :options="options" />
        </div>
      </div>
    </div>
    <div class="input-container">
      <div class="input-container savings" v-if="selected == 'savings'">
        <b-input-group>
          <b-form-input v-model="originalPrice" placeholder="0.00"></b-form-input>
        </b-input-group>
        <b-input-group>
          <b-form-input v-model="discountedPrice" placeholder="0.00"></b-form-input>
        </b-input-group>
      </div>
      <div class="input-container discount" v-if="selected == 'discount'">
        <b-input-group>
          <b-form-input v-model="originalPrice" placeholder="0.00"></b-form-input>
        </b-input-group>
        <b-input-group>
          <b-form-input v-model="percentageDiscount" placeholder="0.00"></b-form-input>
        </b-input-group>
      </div>
      <div class="input-container original" v-if="selected == 'original'">
        <b-input-group>
          <b-form-input v-model="discountedPrice" placeholder="0.00"></b-form-input>
        </b-input-group>
        <b-input-group>
          <b-form-input v-model="percentageDiscount" placeholder="0.00"></b-form-input>
        </b-input-group>
      </div>
    </div>
    <div class="output-container">
      <div class="row">
        <div class="col">
          <div class="tagline">
            {{ outputTagline }}
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div v-if="selected == 'savings'" class="main-output">
            {{ percentageDiscount }}%
          </div>
          <div v-if="selected == 'discount'" class="main-output">
            {{ discountedPrice }}
          </div>
          <div v-if="selected == 'original'" class="main-output">
            {{ originalPrice }}
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col">or</div>
      </div>
      <div class="row">
        <div class="col">
          <div class="main-output">
            {{ savings }}
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
      discountedPrice: 160,
      originalPrice: 200,
      percentageDiscount: 20,
      selected: 'savings',
      savings: 0,
      outputTagline: 'My discount is',
      options: [
        { value: 'savings', text: 'how much discount will I get' },
        { value: 'discount', text: 'how much the discounted price is' },
        { value: 'original', text: 'how much the original price is' },
      ],
    };
  },
  mounted() {},
  methods: {
    computeDiscountedPrice() {
      return this.originalPrice - (this.originalPrice * (this.percentageDiscount / 100));
    },
    computeOriginalPrice() {
      return this.discountedPrice / (1 - this.percentageDiscount / 100);
    },
    computeDiscount() {
      return Math.round((1 - this.discountedPrice / this.originalPrice) * 100);
    },
    computeSavings() {
      return this.originalPrice - this.discountedPrice;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  margin: 72px auto 0 auto;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
